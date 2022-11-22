<script>
 
  import { ethers } from 'ethers';
  import { abi, address } from './contractData.js';
 
  let connectedAccount = "";
  let contractMessage = "";
  let new_message = "";

  const provider = new ethers.providers.Web3Provider(window.ethereum);

  async function connectWallet() {
    if (window.ethereum) {
      await window.ethereum.request({ method: 'eth_requestAccounts' })
      const signer = provider.getSigner();
      connectedAccount = await signer.getAddress();
    } else {
      alert('You need to install metamask');
    }
  }

  async function callContract() {
    const contract = new ethers.Contract(address, abi, provider);
    contractMessage = await contract.message();
  }

  async function setMessage() {
    const signer = provider.getSigner();
    const contract = new ethers.Contract(address, abi, signer);
    await contract.setMessage(new_message);
  }

</script>
 
<main>
  <div>Connected Account: {connectedAccount}</div>
  <div>Contract Message: { contractMessage }</div>
 
  <button on:click={connectWallet}>Connect wallet</button>
  <button on:click={callContract}>Call Contract</button>
  <button on:click={setMessage}>Set Message</button>
  <input type="text"  bind:value={new_message}>
</main>
