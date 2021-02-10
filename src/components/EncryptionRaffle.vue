<template>
  <div>
    <p>This application finds the 5 digit salt value for a given encrypted string generated using MD5</p>
    <label for='email'>Enter the non salted value: </label>
    <input type='text' id='email' v-model='email' />
    <br/>
    <label for='initialString'>Enter the encrypted string:</label>
    <input type='text' id='initialString' v-model='initialString' />
    <p>Press submit button to attempt to solve the encryption</p>

    <button type='submit' @click='useMD5'>Use md5 hash</button>
    <p v-if='raffleString != ""' >Here is the salt value for entry: {{ raffleString }}</p>
  </div>
  
</template>

<script>
import md5  from 'crypto-js/md5'
import Base64 from 'crypto-js/enc-base64';
// algo's # => [:SHA2, :MD5, :SHA256, :SHA384, :SHA512]
// initialString: '3SDrDSrFJCGWfq2ksuszwQ==',
export default {
  mounted() {
    this.findPermutations();
    console.log('raffleCandidates is filled');
  },
  data() {
    return {
      initialString: '',
      email: '',
      raffleString: '',
      raffleCandidates: [],
    }
  },
  methods: {
    findPermutations() {
      const alphabet = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
      const n = alphabet.length;
      const k = 5;
      this.permute(alphabet, '', n, k);
    },
    permute(set, prefix, n, k) {
      if (k === 0) {
        // Exit condition
        this.raffleCandidates.push(prefix);
        return;
      }

      for (let i = 0; i < n; i++) {
        let newPrefix = prefix + set[i];
        this.permute(set, newPrefix, n, k - 1);
      }
    },
    useMD5() {
      console.log('processing MD5....')
      // For each raffle candidate, hash the email + release candidate and then compare to the initial string. Return raffle candidate if matched.
      for(let raffleCandidate of this.raffleCandidates) {
        let encryptedString = Base64.stringify(md5(this.email + raffleCandidate));
        if (encryptedString == this.initialString) {
          console.log('GOT A MATCH!!!!')
          this.raffleString = raffleCandidate;
          break;
        }
      }
      console.log('end processing MD5....');
    },
  }
}
</script>

<style>

</style>