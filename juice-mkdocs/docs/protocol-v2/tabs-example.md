# Protocol v2 Test

=== "Step by step"

    **Create a new project for the specified owner, which mints an NFT (ERC-721) into their wallet.**

    _Anyone can create a project on an owner's behalf._

    #### Definition

    ```solidity
    function createFor(address _owner, JBProjectMetadata calldata _metadata)
      external
      override
      returns (uint256 projectId) { ... }
    ```

    - Arguments:
      - `_owner` is the address that will be the owner of the project.
      - `_metadata` is a struct containing metadata content about the project, and domain within which the metadata applies.
    - The function can be accessed externally by anyone.
    - The function overrides a function definition from the [`IJBProjects`](../../../interfaces/ijbprojects.md) interface.
    - The function returns the token ID of the newly created project.

=== "Code"

    ```solidity
    /**
      @notice
      Create a new project for the specified owner, which mints an NFT (ERC-721) into their wallet.

      @dev
      Anyone can create a project on an owner's behalf.

      @param _owner The address that will be the owner of the project.
      @param _metadata A struct containing metadata content about the project, and domain within which the metadata applies.

      @return projectId The token ID of the newly created project.
    */
    function createFor(address _owner, JBProjectMetadata calldata _metadata)
      external
      override
      returns (uint256 projectId)
    {
      // Increment the count, which will be used as the ID.
      projectId = ++count;

      // Mint the project.
      _safeMint(_owner, projectId);

      // Set the metadata if one was provided.
      if (bytes(_metadata.content).length > 0)
        metadataContentOf[projectId][_metadata.domain] = _metadata.content;

      emit Create(projectId, _owner, _metadata, msg.sender);
    }
    ```

=== "Events"

    | Name                                | Data                                                                                                                                                                                                                                                  |
    | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | [**`Create`**](../events/create.md)                                                                          | <ul><li><code>uint256 indexed projectId</code></li><li><code>address indexed owner</code></li><li><code>[`JBProjectMetadata`](../../../data-structures/jbprojectmetadata.md)metadata</code></li><li><code>address caller</code></li></ul>

=== "Bug bounty"

    | Category          | Description                                                                                                                            | Reward |
    | ----------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------ |
    | **Optimization**  | Help make this operation more efficient.                                                                                               | 0.5ETH |
    | **Low severity**  | Identify a vulnerability in this operation that could lead to an inconvenience for a user of the protocol or for a protocol developer. | 1ETH   |
    | **High severity** | Identify a vulnerability in this operation that could lead to data corruption or loss of funds.                                        | 5+ETH  |
