# \_isApproved

{% tabs %}
{% tab title="Step by step" %}

{% endtab %}

{% tab title="Only code" %}
```javascript
/** 
  @notice 
  Checks to see if the provided funding cycle is approved according to the correct ballot.

  @param _fundingCycle The ID of the funding cycle to get an approval flag for.

  @return The approval flag.
*/
function _isApproved(JBFundingCycle memory _fundingCycle) private view returns (bool) {
  return
    _ballotStateOf(_fundingCycle.id, _fundingCycle.configured, _fundingCycle.basedOn) ==
    JBBallotState.Approved;
}
```
{% endtab %}

{% tab title="Bug bounty" %}
| Category | Description | Reward |
| :--- | :--- | :--- |
| **Optimization** | Help make this operation more efficient. | 0.5ETH |
| **Low severity** | Identify a vulnerability in this operation that could lead to an inconvenience for a user of the protocol or for a protocol developer. | 1ETH |
| **High severity** | Identify a vulnerability in this operation that could lead to data corruption or loss of funds. | 5+ETH |
{% endtab %}
{% endtabs %}
