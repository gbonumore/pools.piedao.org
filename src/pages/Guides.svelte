<script>
  import BigNumber from "bignumber.js";
  import images from "../config/images.json";
  import FarmerTable from '../components/FarmerTable.svelte';
  import { farming } from '../stores/eth/writables.js';
  import {
    formatFiat,
    toFixed,
    subscribeToBalance,
  } from "../components/helpers.js";
   import {
    balanceKey,
    balances,
    contract,
  } from "../stores/eth.js";
  
  const DOUGH = '0xad32A8e6220741182940c5aBF610bDE99E737b2D';
  const DAO = '0x4efD8CEad66bb0fA64C8d53eBE65f31663199C6d';
  const MULTISIG = '0x3bFdA5285416eB06Ebc8bc0aBf7d105813af06d0';

  let daoBalanceKey;
  let msBalanceKey;
  let doughStaked;
  let price = 'n/a';
  let circulatingSupply = 0;
  

  const addToken = () => {
    ethereum.sendAsync({
        method: 'wallet_watchAsset',
        params: {
          "type":"ERC20",
          "options":{
            "address": '0xad32A8e6220741182940c5aBF610bDE99E737b2D',
            "symbol": 'DOUGH',
            "decimals": 18,
            "image": 'https://raw.githubusercontent.com/pie-dao/brand/master/DOUGH%20Token/DOUGH2v.png',
          },
        },
        id: Math.round(Math.random() * 100000),
    }, (err, added) => {
      if (added) {
        console.log('Thanks for your interest!')
      } else {
        alert('Something went wrong. Is Metamask there?')
      }
    })
  };
  
  $: (async () => {

    const doughToken = await contract({ address: DOUGH });
    // DAO
    subscribeToBalance(DOUGH, DAO, true);
    // Multisig
    subscribeToBalance(DOUGH, MULTISIG, true);

    daoBalanceKey = balanceKey(DOUGH, DAO);
    msBalanceKey = balanceKey(DOUGH, MULTISIG);

    const totalSupply = await doughToken.totalSupply();
    const daoBal = $balances[daoBalanceKey] || BigNumber(0);
    const msBal = $balances[msBalanceKey] || BigNumber(0);
    doughStaked = $farming['0x8314337d2b13e1A61EadF0FD1686b2134D43762F'].doughStaked.toFixed(2) || 0;
    price = $farming['0x8314337d2b13e1A61EadF0FD1686b2134D43762F'].DOUGHPrice.toFixed(2) || 0;

    if(daoBal > 0 && msBal > 0) {
      const ts = BigNumber(totalSupply.toString()).dividedBy(10**18)
      circulatingSupply = ts.minus(BigNumber(msBal)).minus(BigNumber(daoBal)).toFixed(2);
    }
  })()
</script>
  <div class="content flex flex-col spl">

Guide


</div>



  