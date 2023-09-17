---
layout: post
title: How does contract deployment works with an EOA?
date: '2023-09-16'
summary: Demistifying the details of contract deployment from EOA
---

I encountered this question and attempted to find an answer on the internet, but I couldn't find a straightforward explanation. Does it simply "work" like magic?

To create a contract, we must send the contract's bytecode to the zero address, known as `0x0000000000000000000000000000000000000000`. However, how does this process unfold afterward? The zero address isn't a contract itself, so how does it utilize the bytecode we send to deploy the contract?

As it turns out, even though the zero address is an Externally Owned Account (EOA), the nodes don't process transactions to it like they do with other EOAs.

<ol>
<li>
The function responsible for applying a new transaction to the state, first checks whether the transaction is sent to the zero address. If it is, it sets the <code>contractCreation</code> value to <i>true</i>.

{% highlight golang %}
var (
	msg              = st.msg
	sender           = vm.AccountRef(msg.From)
	rules            = st.evm.ChainConfig().Rules(st.evm.Context.BlockNumber, st.evm.Context.Random != nil, st.evm.Context.Time)
	contractCreation = msg.To == nil
)
{% endhighlight %}

</li>
<li>
If the transaction is sent to the zero address (i.e., <code>contractCreation</code> is true), instead of invoking the <code>call</code> function in EVM, the <code>create</code> function is called with the <i>msg.data</i>

{% highlight golang %}
if contractCreation {
	ret, _, st.gasRemaining, vmerr = st.evm.Create(sender, msg.Data, st.gasRemaining, msg.Value)
} else {
	st.state.SetNonce(msg.From, st.state.GetNonce(sender.Address())+1)
	ret, st.gasRemaining, vmerr = st.evm.Call(sender, st.to(), msg.Data, st.gasRemaining, msg.Value)
}
{% endhighlight %}

</li>
<li>Subsequently, this bytecode is executed to obtain the runtime code.

{% highlight golang %}
contract := NewContract(caller, AccountRef(address), value, gas)
contract.SetCodeOptionalHash(&address, bytecode)
runtimeCode, err := evm.interpreter.Run(contract, nil, false)

{% endhighlight %}

</li>
<li>And then after a gas check, this runtime code is saved at the address that has already been generated.

{% highlight golang %}
if err == nil {
	createDataGas := uint64(len(ret)) * params.CreateDataGas
	if contract.UseGas(createDataGas) {
		evm.StateDB.SetCode(address, runtimeCode)
	} else {
		err = ErrCodeStoreOutOfGas
	}
}
{% endhighlight %}
</li>
</ol>

Kaching!!, your contract is deployed.

**_NOTE:_** code snippets are references from geth.