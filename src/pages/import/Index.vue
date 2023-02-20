<template>
  <q-page class="">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->
    <div class="q-py-sm q-px-sm">
      <q-btn color="primary" :label="currentType">
        <q-menu>
          <q-list style="min-width: 100px">
            <q-item clickable v-close-popup @click="switchType('Phrase')" :active="currentType === 'Phrase'">
              <q-item-section>Phrase</q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable v-close-popup  @click="switchType('Other')" :active="currentType === 'Other'">
              <q-item-section>Other</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
      </q-btn>
      <q-input filled clearable v-model="phrase" class="text-subtitle2 q-mt-sm" type="textarea" :placeholder="placeholder" hint=""></q-input>
      <div class="row item-center justify-center">
        <q-btn class="q-mt-sm full-width" color="primary" label="Import" @click="improt"/>
      </div>

      <div class="q-my-sm">
        <q-card dark bordered class="bg-grey-9">
          <q-card-section>
            <div class="text-h6">result:</div>
          </q-card-section>

          <q-separator dark inset />

          <q-card-section style="white-space:pre-line;word-wrap:break-word;display:block;width:auto;word-break:normal;overflow: hidden;">
            {{ result }}
          </q-card-section>
        </q-card>
      </div>
    </div>

  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
const {generateMasterKeys} = require('@signumjs/crypto')
const {Address} = require('@signumjs/core')

export default defineComponent({
  name: 'Import',
  data() {
    return {
      phrase:null,
      result:null,
      placeholder:'Import from here,support Phrase',
      currentType:'Phrase'
    }
  },
  methods:{
    improt() {
      let _ = this
      if (!(_.phrase && _.phrase.length)) {
        return
      }
      // const keys = generateMasterKeys(_.phrase)
      const accountAddress = Address.create(_.phrase,'TS')
      _.result = ''
      _.result += 'Public Key: ' + accountAddress.getPublicKey() + '\n'
      console.log(_.result)
      _.result += 'Numeric Id: ' + accountAddress.getNumericId() + '\n'
      console.log(_.result)
      _.result += 'Address: ' +  accountAddress.getReedSolomonAddress('TS') + '\n'
      console.log(_.result)
    },
    switchType(input) {
      console.log('switchType',input)
      let _ = this
      _.currentType = input
      _.placeholder = _.currentType == 'Phrase' ? 'Import from here,support Phrase' : 'Import from here,support numeric ID, or public key，or Reed Solomon address'
    },
  }
})
</script>
