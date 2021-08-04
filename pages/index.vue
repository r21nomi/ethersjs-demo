<template>
  <div class="container">
    <div>
      <button @click="onConnectButtonClicked">Connect to MetaMaks</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import { ethers } from "ethers"
@Component({
  components: {
  },
})
export default class TopPage extends Vue {
  private isMetaMaskInstalled(): boolean {
    const { ethereum } = (window as any);
    return ethereum && ethereum.isMetaMask;
  };

  private async onConnectButtonClicked() {
    if (!this.isMetaMaskInstalled()) {
      window.alert('MetaMask not found.')
      return
    }
    const { ethereum } = (window as any)
    try {
      const newAccounts = await ethereum.request({
        method: 'eth_requestAccounts',
      })
      console.log(newAccounts)
      const provider = new ethers.providers.Web3Provider((window as any).ethereum)
      const signer = provider.getSigner()
      // const h = ethers.utils.arrayify(new TextEncoder().encode("Hello World"))
      // const h = ethers.utils.keccak256(ethers.utils.toUtf8Bytes("Hello World"))
      // const h = ethers.utils.toUtf8Bytes(h1);
      const message = "Hello_World_xxx"
      const signature = await signer.signMessage(message)
      console.log(signature)
      // let recoveredAddress = ethers.utils.recoverAddress(h, signature)
      // console.log(recoveredAddress)

      // TODO: Call API
      // http://localhost:9000/v1/auth/[signature]?message=[message]&address=[address]
    } catch (e) {
      console.log(e)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
