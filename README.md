# English Auction

#### NFT 옥션 제작 실습

## 목표
1. NFT 판매자가 이 계약을 배포합니다.
2. 경매는 7일 동안 진행됩니다.
3. 참가자들은 현재 최고 입찰자보다 많은 ETH를 입금함으로써 입찰할 수 있습니다.
4. 최고 입찰자가 아닌 모든 입찰자는 자신의 입찰금을 철회할 수 있습니다.
5. 옥션 종료 후 최고 입찰자가 NFT의 새로운 소유자가 됩니다.
6. 판매자는 최고 입찰가의 ETH를 받습니다.

## Remix 세팅

#### 1. Remix 접속
> Remix: <https://remix.ethereum.org/>
#### 2. Workspace 생성
> 1. 왼쪽 상단의 workspaces 클릭 후 Create 클릭 <br><br>
> <img src="https://github.com/Joon2000/English-Auction-Solidity/blob/main/images/workspaces.png" width="250" height="400"></img><br><br><br>
> 2. Choose a template을 Blank 선택 Workspace name에 workspace 이름 적기<br><br>
> <img src="https://github.com/Joon2000/English-Auction-Solidity/blob/main/images/Create%20workspace.png" width="500" height="300"></img><br><br><br>
3. 모든 폴더와 파일 삭제<br><br>
4. contracts 폴더와 solidity 파일 생성<br><br>
> <img src="https://github.com/Joon2000/English-Auction-Solidity/blob/main/images/%E1%84%85%E1%85%A9%E1%86%AF%E1%84%83%E1%85%A5%20%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC.png" width="250" height="400"></img><br><br><br>

## 미션 진행

#### 1. lisence와 solidity 컴파일 버전 설정
> lisence: MIT<br>
> 컴파일 버전: ^0.8.24<br>
 ```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;
```

#### 2. IERC-721 interface 
> IERC721의 safeTransferFrom과 transferFrom 함수를 사용할 것입니다.
```solidity
interface IERC721 {
    function safeTransferFrom(address from, address to, uint256 tokenId)
        external;
    function transferFrom(address, address, uint256) external;
}
```

#### 3. contract 생성
> EnglishAuction contract 생성
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

interface IERC721 {
    function safeTransferFrom(address from, address to, uint256 tokenId)
        external;
    function transferFrom(address, address, uint256) external;
}

contract EnglishAuction {}
```

#### 4. event 설정
> 4개의 event를 만들 것입니다.
> 1. Start(): 경매가 시작되었음을 알리는 이벤트.
> 2. Bid(address indexed sender, uint256 amount): 주소 sender에서 금액 amount으로 새로운 입찰이 이루어졌음을 알리는 이벤트.
> 3. Withdraw(address indexed bidder, uint256 amount): 주소 bidder가 입찰 금액 amount을 철회함을 알리는 이벤트.
> 4. End(address winner, uint256 amount): 경매가 종료되었으며, winner가 최고 입찰 금액 amount로 승리함을 알리는 이벤트. <br><br>
```solidity
event Start();
event Bid(address indexed sender, uint256 amount);
event Withdraw(address indexed bidder, uint256 amount);
event End(address winner, uint256 amount);
```

#### 5. 전역변수 설정
