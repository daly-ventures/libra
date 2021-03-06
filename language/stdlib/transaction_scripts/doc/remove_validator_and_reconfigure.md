
<a name="remove_validator_and_reconfigure"></a>

# Script `remove_validator_and_reconfigure`



-  [Summary](#@Summary_0)
-  [Technical Description](#@Technical_Description_1)
-  [Parameters](#@Parameters_2)
-  [Common Abort Conditions](#@Common_Abort_Conditions_3)
-  [Related Scripts](#@Related_Scripts_4)


<a name="@Summary_0"></a>

## Summary

This script removes a validator account from the validator set, and triggers a reconfiguration
of the system to remove the validator from the system. This transaction can only be
successfully called by the Libra Root account.


<a name="@Technical_Description_1"></a>

## Technical Description

This script removes the account at <code>validator_address</code> from the validator set. This transaction
emits a <code><a href="../../modules/doc/LibraConfig.md#0x1_LibraConfig_NewEpochEvent">LibraConfig::NewEpochEvent</a></code> event. Once the reconfiguration triggered by this event
has been performed, the account at <code>validator_address</code> is no longer considered to be a
validator in the network. This transaction will fail if the validator at <code>validator_address</code>
is not in the validator set.


<a name="@Parameters_2"></a>

## Parameters

| Name                | Type         | Description                                                                                                                        |
| ------              | ------       | -------------                                                                                                                      |
| <code>lr_account</code>        | <code>&signer</code>    | The signer reference of the sending account of this transaction. Must be the Libra Root signer.                                    |
| <code>sliding_nonce</code>     | <code>u64</code>        | The <code>sliding_nonce</code> (see: <code><a href="../../modules/doc/SlidingNonce.md#0x1_SlidingNonce">SlidingNonce</a></code>) to be used for this transaction.                                                         |
| <code>validator_name</code>    | <code>vector&lt;u8&gt;</code> | ASCII-encoded human name for the validator. Must match the human name in the <code><a href="../../modules/doc/ValidatorConfig.md#0x1_ValidatorConfig_ValidatorConfig">ValidatorConfig::ValidatorConfig</a></code> for the validator. |
| <code>validator_address</code> | <code>address</code>    | The validator account address to be removed from the validator set.                                                                |


<a name="@Common_Abort_Conditions_3"></a>

## Common Abort Conditions

| Error Category             | Error Reason                            | Description                                                                                     |
| ----------------           | --------------                          | -------------                                                                                   |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_INVALID_ARGUMENT">Errors::INVALID_ARGUMENT</a></code> | <code><a href="../../modules/doc/SlidingNonce.md#0x1_SlidingNonce_ENONCE_TOO_OLD">SlidingNonce::ENONCE_TOO_OLD</a></code>          | The <code>sliding_nonce</code> is too old and it's impossible to determine if it's duplicated or not.      |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_INVALID_ARGUMENT">Errors::INVALID_ARGUMENT</a></code> | <code><a href="../../modules/doc/SlidingNonce.md#0x1_SlidingNonce_ENONCE_TOO_NEW">SlidingNonce::ENONCE_TOO_NEW</a></code>          | The <code>sliding_nonce</code> is too far in the future.                                                   |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_INVALID_ARGUMENT">Errors::INVALID_ARGUMENT</a></code> | <code><a href="../../modules/doc/SlidingNonce.md#0x1_SlidingNonce_ENONCE_ALREADY_RECORDED">SlidingNonce::ENONCE_ALREADY_RECORDED</a></code> | The <code>sliding_nonce</code> has been previously recorded.                                               |
| EMPTY                      | 0                                       | The provided <code>validator_name</code> does not match the already-recorded human name for the validator. |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_INVALID_ARGUMENT">Errors::INVALID_ARGUMENT</a></code> | <code><a href="../../modules/doc/LibraSystem.md#0x1_LibraSystem_ENOT_AN_ACTIVE_VALIDATOR">LibraSystem::ENOT_AN_ACTIVE_VALIDATOR</a></code> | The validator to be removed is not in the validator set.                                        |
| <code><a href="../../modules/doc/Errors.md#0x1_Errors_REQUIRES_ADDRESS">Errors::REQUIRES_ADDRESS</a></code> | <code><a href="../../modules/doc/CoreAddresses.md#0x1_CoreAddresses_ELIBRA_ROOT">CoreAddresses::ELIBRA_ROOT</a></code>            | The sending account is not the Libra Root account.                                              |


<a name="@Related_Scripts_4"></a>

## Related Scripts

* <code><a href="create_validator_account.md#create_validator_account">Script::create_validator_account</a></code>
* <code><a href="create_validator_operator_account.md#create_validator_operator_account">Script::create_validator_operator_account</a></code>
* <code><a href="register_validator_config.md#register_validator_config">Script::register_validator_config</a></code>
* <code><a href="add_validator_and_reconfigure.md#add_validator_and_reconfigure">Script::add_validator_and_reconfigure</a></code>
* <code><a href="set_validator_operator.md#set_validator_operator">Script::set_validator_operator</a></code>
* <code><a href="set_validator_operator_with_nonce_admin.md#set_validator_operator_with_nonce_admin">Script::set_validator_operator_with_nonce_admin</a></code>
* <code><a href="set_validator_config_and_reconfigure.md#set_validator_config_and_reconfigure">Script::set_validator_config_and_reconfigure</a></code>


<pre><code><b>public</b> <b>fun</b> <a href="remove_validator_and_reconfigure.md#remove_validator_and_reconfigure">remove_validator_and_reconfigure</a>(lr_account: &signer, sliding_nonce: u64, validator_name: vector&lt;u8&gt;, validator_address: address)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>fun</b> <a href="remove_validator_and_reconfigure.md#remove_validator_and_reconfigure">remove_validator_and_reconfigure</a>(
    lr_account: &signer,
    sliding_nonce: u64,
    validator_name: vector&lt;u8&gt;,
    validator_address: address
) {
    <a href="../../modules/doc/SlidingNonce.md#0x1_SlidingNonce_record_nonce_or_abort">SlidingNonce::record_nonce_or_abort</a>(lr_account, sliding_nonce);
    <b>assert</b>(<a href="../../modules/doc/ValidatorConfig.md#0x1_ValidatorConfig_get_human_name">ValidatorConfig::get_human_name</a>(validator_address) == validator_name, 0);
    <a href="../../modules/doc/LibraSystem.md#0x1_LibraSystem_remove_validator">LibraSystem::remove_validator</a>(lr_account, validator_address);
}
</code></pre>



</details>
