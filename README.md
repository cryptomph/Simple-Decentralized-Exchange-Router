# Simple-Decentralized-Exchange-Router
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.0/contracts/token/ERC20/IERC20.sol";

contract SimpleDEXRouter {
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to
    ) external {
        require(path.length == 2, "Invalid path");
        IERC20 tokenIn = IERC20(path[0]);
        IERC20 tokenOut = IERC20(path[1]);

        tokenIn.transferFrom(msg.sender, address(this), amountIn);

        // Simple 1% slippage simulation
        uint256 amountOut = (amountIn * 99) / 100;
        require(amountOut >= amountOutMin, "Slippage too high");

        tokenOut.transfer(to, amountOut);
    }
}
