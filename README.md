## NTF CODE
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721URIStorage, Ownable {
    string public baseTokenURI;

    constructor(string memory _name, string memory _symbol, string memory _baseTokenURI) ERC721(_name, _symbol) {
        baseTokenURI = _baseTokenURI;
    }

    function mint(address to, uint256 tokenId) public onlyOwner {
        _mint(to, tokenId);
    }
}

```

## NTF MARKETPLACE
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/IERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract NFTMarket is Ownable {
    IERC20 public erc20Token;
    IERC721 public nftContract;

    constructor(address _erc20Token, address _nftContract) {
        erc20Token = IERC20(_erc20Token);
        nftContract = IERC721(_nftContract);
    }

    function buyNFT(uint256 tokenId, uint256 price) public {
        require(erc20Token.allowance(msg.sender, address(this)) >= price, "Allowance not set");
        require(erc20Token.balanceOf(msg.sender) >= price, "Insufficient balance");

        erc20Token.transferFrom(msg.sender, owner(), price);
        nftContract.transferFrom(owner(), msg.sender, tokenId);
    }

    function setNFTContract(address _nftContract) public onlyOwner {
        nftContract = IERC721(_nftContract);
    }

    function setERC20Token(address _erc20Token) public onlyOwner {
        erc20Token = IERC20(_erc20Token);
    }
}
```

**¿Qué caso de uso pretende resolver tu (colección) NFT?**

El caso de uso de esta colección NFT podría ser la creación de una plataforma que permita a los artistas emitir NFT para sus obras de arte digitales. Los artistas pueden utilizar el contrato NFT para crear y gestionar sus NFT, y los coleccionistas pueden comprar esos NFT utilizando tokens ERC20 a través del contrato NFTMarket.

**¿Qué valor añadido aporta esta (colección) NFT a las existentes?**

Este conjunto de contratos proporciona una forma de emisión de NFT personalizada y permite la compra de NFT con tokens ERC20. El valor añadido radica en la flexibilidad y personalización que ofrecen estos contratos. Los artistas pueden crear sus propios NFT con metadatos personalizados, y los compradores pueden adquirirlos con tokens ERC20 en lugar de ETH, lo que puede ser más accesible y rentable, y tambien su facilidad de modificar el token con el que pueden ser adquiridos.

**¿Cómo crees que podría mejorarse técnicamente?**

Existen muchas formas de mejorar estos contratos desde el punto de vista técnico, como la implementación de mecanismos de subasta, la gestión de derechos de autor y tambien medidas de seguridad adicional. Además, se pueden aplicar prácticas recomendadas de desarrollo seguro y auditorías para garantizar la robustez de los contratos y la seguridad de los activos digitales. También es importante garantizar una experiencia de usuario fácil y amigable para artistas y coleccionistas. Tambien se le podria añadir el tema de comisiones o porcentaje de ganancia para el creador del marketplace.


# PROBLEMAS
Al momento de subir el codigo a la testnet de goerli me surgieron varios errores con temas de librerias y otras cosas, lo que me impidió seguir continuando con el ejercicio.


