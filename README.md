# Create-a-UI-for-Submitting-PayForBlob-Transactions

#Here is an example of a user interface to send a PayForBlob transaction on the Celestia Network using React and web3.js:

import React, { useState } from "react";
import Web3 from "web3";
import PayForBlobContract from "./contracts/PayForBlob.json";

const web3 = new Web3(window.ethereum);

const PayForBlob = () => {
  const [amount, setAmount] = useState("");
  const [address, setAddress] = useState("");
  const [status, setStatus] = useState("");

  const handleAmountChange = (e) => setAmount(e.target.value);
  const handleAddressChange = (e) => setAddress(e.target.value);

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const accounts = await window.ethereum.request({
        method: "eth_requestAccounts",
      });
      const contractAddress = "0x123..."; // address of the PayForBlob smart contract on Celestia Network
      const contractInstance = new web3.eth.Contract(
        PayForBlobContract.abi,
        contractAddress
      );
      await contractInstance.methods.pay(address).send({
        from: accounts[0],
        value: web3.utils.toWei(amount, "ether"),
      });
      setStatus("Transaction successful!");
    } catch (error) {
      console.error(error);
      setStatus("There was an error sending the transaction.");
    }
  };
  return (
    <div>
      <h1>PayForBlob</h1>
      <form onSubmit={handleSubmit}>
        <label>
          Amount (CELO):
          <input type="text" value={amount} onChange={handleAmountChange} />
        </label>
        <br />
        <label>
          Recipient address:
          <input type="text" value={address} onChange={handleAddressChange} />
        </label>
        <br />
        <button type="submit">Send</button>
      </form>
      <p>{status}</p>
    </div>
  );
};

export default PayForBlob;


Note that in the example above, we used window.ethereum to interact with the Celestia Network through MetaMask. You will need to install and connect MetaMask to the Celestia Network to use this example.
