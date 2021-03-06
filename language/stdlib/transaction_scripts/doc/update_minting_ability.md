
<a name="update_minting_ability"></a>

# Script `update_minting_ability`



-  [Summary](#@Summary_0)
-  [Technical Description](#@Technical_Description_1)
-  [Parameters](#@Parameters_2)
-  [Common Abort Conditions](#@Common_Abort_Conditions_3)
-  [Related Scripts](#@Related_Scripts_4)


<a name="@Summary_0"></a>

## Summary

Script to allow or disallow minting of new coins in a specified currency.  This transaction can
only be sent by the Treasury Compliance account.  Turning minting off for a currency will have
no effect on coins already in circulation, and coins may still be removed from the system.


<a name="@Technical_Description_1"></a>

## Technical Description

This transaction sets the <code>can_mint</code> field of the <code><a href="../../modules/doc/Libra.md#0x1_Libra_CurrencyInfo">Libra::CurrencyInfo</a>&lt;Currency&gt;</code> resource
published under <code>0xA550C18</code> to the value of <code>allow_minting</code>. Minting of coins if allowed if
this field is set to <code><b>true</b></code> and minting of new coins in <code>Currency</code> is disallowed otherwise.
This transaction needs to be sent by the Treasury Compliance account.


<a name="@Parameters_2"></a>

## Parameters

| Name            | Type      | Description                                                                                                                          |
| ------          | ------    | -------------                                                                                                                        |
| <code>Currency</code>      | Type      | The Move type for the <code>Currency</code> whose minting ability is being updated. <code>Currency</code> must be an already-registered currency on-chain. |
| <code>account</code>       | <code>&signer</code> | Signer reference of the sending account. Must be the Libra Root account.                                                             |
| <code>allow_minting</code> | <code>bool</code>    | Whether to allow minting of new coins in <code>Currency</code>.                                                                                 |


<a name="@Common_Abort_Conditions_3"></a>

## Common Abort Conditions

| Error Category             | Error Reason                          | Description                                          |
| ----------------           | --------------                        | -------------                                        |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_REQUIRES_ADDRESS">Errors::REQUIRES_ADDRESS</a></code> | <code><a href="../../modules/doc/CoreAddresses.md#0x1_CoreAddresses_ETREASURY_COMPLIANCE">CoreAddresses::ETREASURY_COMPLIANCE</a></code> | <code>tc_account</code> is not the Treasury Compliance account. |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_NOT_PUBLISHED">Errors::NOT_PUBLISHED</a></code>    | <code><a href="../../modules/doc/Libra.md#0x1_Libra_ECURRENCY_INFO">Libra::ECURRENCY_INFO</a></code>               | <code>Currency</code> is not a registered currency on-chain.    |


<a name="@Related_Scripts_4"></a>

## Related Scripts

* <code>Scripts::update_dual_attestation_limit</code>
* <code>Scripts::update_exchange_rate</code>


<pre><code><b>public</b> <b>fun</b> <a href="update_minting_ability.md#update_minting_ability">update_minting_ability</a>&lt;Currency&gt;(tc_account: &signer, allow_minting: bool)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>fun</b> <a href="update_minting_ability.md#update_minting_ability">update_minting_ability</a>&lt;Currency&gt;(
    tc_account: &signer,
    allow_minting: bool
) {
    <a href="../../modules/doc/Libra.md#0x1_Libra_update_minting_ability">Libra::update_minting_ability</a>&lt;Currency&gt;(tc_account, allow_minting);
}
</code></pre>



</details>
