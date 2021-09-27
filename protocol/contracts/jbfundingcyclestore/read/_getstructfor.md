# \_getStructFor

Contract:[`JBFundingCycleStore`](../)​

{% tabs %}
{% tab title="Step by step" %}
**The currency ballot state of the project.**

Definition:

```javascript
function _getStructFor(uint256 _id) private view returns (JBFundingCycle memory fundingCycle) { ... }
```

* `_id` is the funding cycle ID to get the full struct for.
* The view function is private to this contract.
* The function does not alter state on the blockchain.
* The function returns a [`JBFundingCycle`](../../../data-structures/jbfundingcycle.md) stored in the `fundingCycle` reference.

1. If the `_id` provided is 0, return an empty funding cycle.

   ```javascript
   // Return an empty funding cycle if the ID specified is 0.
   if (_id == 0) return fundingCycle;
   ```

2. Set the funding cycle's id to the provided value.

   ```javascript
   fundingCycle.id = _id;
   ```

3. Get the stored intrinsic properties of the funding cycle. Populate the struct values by de-packing the `uint256`.  


   1. _Internal references:_
      * [`_packedInstrinsicPropertiesOf`](../properties/_packedintrinsicpropertiesof.md)

   ```javascript
   uint256 _packedIntrinsicProperties = _packedIntrinsicPropertiesOf[_id];

   fundingCycle.weight = uint256(uint80(_packedIntrinsicProperties));
   fundingCycle.projectId = uint256(uint56(_packedIntrinsicProperties >> 80));
   fundingCycle.basedOn = uint256(uint48(_packedIntrinsicProperties >> 136));
   fundingCycle.start = uint256(uint48(_packedIntrinsicProperties >> 184));
   fundingCycle.number = uint256(uint24(_packedIntrinsicProperties >> 232));
   ```

4. Get the stored configuration properties of the funding cycle. Populate the struct values by de-packing the `uint256`.  


   1. _Internal references:_
      * [`_packedConfigurationPropertiesOf`](../properties/_packedconfigurationpropertiesof.md)

   ```javascript
   uint256 _packedConfigurationProperties = _packedConfigurationPropertiesOf[_id];
  
   fundingCycle.ballot = IJBFundingCycleBallot(address(uint160(_packedConfigurationProperties)));
   fundingCycle.configured = uint256(uint48(_packedConfigurationProperties >> 160));
   fundingCycle.duration = uint256(uint16(_packedConfigurationProperties >> 208));
   fundingCycle.currency = uint256(uint8(_packedConfigurationProperties >> 224));
   fundingCycle.fee = uint256(uint8(_packedConfigurationProperties >> 232));
   fundingCycle.discountRate = uint256(uint8(_packedConfigurationProperties >> 240));
   fundingCycle.cycleLimit = uint256(uint8(_packedConfigurationProperties >> 248));  
   ```

5. Populate the `target` property of the struct by reading from what's stored in `_targetOf`.  


   1. _Internal references:_
      * [`_targetOf`](../properties/_targetof.md)

   ```javascript
   fundingCycle.target = _targetOf[_id];
   ```

6. Populate the `tapped` property of the struct by reading from what's stored in `_tappedAmountOf`.  


   1. _Internal references:_
      * [`_tappedAmountOf`](../properties/_targetof.md)

   ```javascript
   fundingCycle.tapped = _tappedAmountOf[_id];
   ```

7. Populate the `metadata` property of the struct by reading from what's stored in `_metadataOf`.  


   1. _Internal references:_
      * [`_metadataOf`](../properties/_metadataof.md)

   ```javascript
   fundingCycle.metadata = _metadataOf[_id];
   ```

.
{% endtab %}

{% tab title="Only code" %}
```javascript
/**
  @notice 
  Unpack a funding cycle's packed stored values into an easy-to-work-with funding cycle struct.

  @param _id The funding cycle ID to get the full struct for.

  @return _fundingCycle The funding cycle struct.
*/
function _getStructFor(uint256 _id) private view returns (JBFundingCycle memory fundingCycle) {
  // Return an empty funding cycle if the ID specified is 0.
  if (_id == 0) return fundingCycle;

  fundingCycle.id = _id;

  uint256 _packedIntrinsicProperties = _packedIntrinsicPropertiesOf[_id];

  fundingCycle.weight = uint256(uint80(_packedIntrinsicProperties));
  fundingCycle.projectId = uint256(uint56(_packedIntrinsicProperties >> 80));
  fundingCycle.basedOn = uint256(uint48(_packedIntrinsicProperties >> 136));
  fundingCycle.start = uint256(uint48(_packedIntrinsicProperties >> 184));
  fundingCycle.number = uint256(uint24(_packedIntrinsicProperties >> 232));

  uint256 _packedConfigurationProperties = _packedConfigurationPropertiesOf[_id];
  
  fundingCycle.ballot = IJBFundingCycleBallot(address(uint160(_packedConfigurationProperties)));
  fundingCycle.configured = uint256(uint48(_packedConfigurationProperties >> 160));
  fundingCycle.duration = uint256(uint16(_packedConfigurationProperties >> 208));
  fundingCycle.currency = uint256(uint8(_packedConfigurationProperties >> 224));
  fundingCycle.fee = uint256(uint8(_packedConfigurationProperties >> 232));
  fundingCycle.discountRate = uint256(uint8(_packedConfigurationProperties >> 240));
  fundingCycle.cycleLimit = uint256(uint8(_packedConfigurationProperties >> 248));
  
  fundingCycle.target = _targetOf[_id];
  fundingCycle.tapped = _tappedAmountOf[_id];
  fundingCycle.metadata = _metadataOf[_id];
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
