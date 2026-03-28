# Soulbound-Token-Non-Transferable-NFT-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract SoulboundToken is ERC721, Ownable {
    constructor(string memory name, string memory symbol, address initialOwner)
        ERC721(name, symbol)
        Ownable(initialOwner)
    {}

    function mint(address to, uint256 tokenId) external onlyOwner {
        _safeMint(to, tokenId);
    }

    // Override transfer functions to make it soulbound
    function _update(address to, uint256 tokenId, address auth) internal override returns (address) {
        require(to == address(0) || ownerOf(tokenId) == address(0), "Soulbound: Transfer not allowed");
        return super._update(to, tokenId, auth);
    }

    function burn(uint256 tokenId) external {
        require(ownerOf(tokenId) == msg.sender, "Not owner");
        _burn(tokenId);
    }
}
