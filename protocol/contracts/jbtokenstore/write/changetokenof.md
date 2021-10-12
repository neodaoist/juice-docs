# changeTokenOf

Contract:[`JBTokenStore`](../)​‌

Interface: `IJBTokenStore`

{% tabs %}
{% tab title="Step by step" %}
**Swap the current project's token that is minted and burned for another, and transfer ownership from the current to another address.**

_Only a project owner or operator can change its token._

Definition:

```solidity
function changeTokenOf(
  uint256 _projectId,
  IJBToken _token,
  address _newOwner
)
  external
  override
  requirePermission(projects.ownerOf(_projectId), _projectId, JBOperations.CHANGE_TOKEN) { ... }
```

* `_projectId` is the ID of the project to which the changed token belongs.
* `_token` is the new token.
* `_newOwner` is an address to transfer the current token's ownership to. This is optional, but it cannot be done later.
* Through the [`requirePermission`](../../jboperatable/modifiers/requirepermission.md) modifier, the function is only accessible by the project's owner, or from an operator that has been given the `JBOperations.CHANGE_TOKEN`permission by the project owner for the provided `_projectId`.
* The function overrides a function definition from the `IJBTokenStore` interface.
* The function returns nothing.
{% endtab %}

{% tab title="Only code" %}
```solidity
/**
  @notice 
  Swap the current project's token that is minted and burned for another, and transfer ownership from the current to another address.

  @dev
  Only a project owner or operator can change its token.

  @param _projectId The ID of the project to which the changed token belongs.
  @param _token The new token.
  @param _newOwner An address to transfer the current token's ownership to. This is optional, but it cannot be done later.
*/
function changeTokenOf(
  uint256 _projectId,
  IJBToken _token,
  address _newOwner
)
  external
  override
  requirePermission(projects.ownerOf(_projectId), _projectId, JBOperations.CHANGE_TOKEN)
{
  // Get a reference to the current owner of the token.
  IJBToken _currentToken = tokenOf[_projectId];

  // Store the new token.
  tokenOf[_projectId] = _token;

  // If a new owner was provided, transfer ownership of the old token to the new owner.
  if (_newOwner != address(0)) _currentToken.transferOwnership(_newOwner);

  emit ChangeToken(_projectId, _token, _newOwner, msg.sender);
}
```
{% endtab %}

{% tab title="Bug bounty" %}

{% endtab %}
{% endtabs %}
