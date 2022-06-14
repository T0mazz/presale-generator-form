<template>
  <q-page id="prs-gen-frm" class="row items-baseline justify-evenly">
    <div class="form-container col-12">
        <div class="form-header">
          <span class="header-text">Create your presale</span>
        </div>
        <q-form
          @submit.prevent=""
          @reset.prevent=""
          ref="presaleGeneratorForm"
          class=""
          id="pg-form"
        >
          <div id="step-1" class="header-filler"></div>
          <div class="step-header">
            <span v-if="!isDesktop" class="step-text">Step {{currentStep}}</span>
            <span v-if="isDesktop" class="step-text">Step 1</span>
          </div>
          <div v-show="currentStep > 1 && isDesktop || currentStep === 1" class="step-content q-pa-md">
            <div class="wllt-inpts frm-input">
              <q-input class="sm-inpt-left wdth-70" outlined filled v-model="tokenAddres" label="Token address" dark/>
              <q-card flat class="my-card text-lightgrey bg-transparent sm-inpt-right wdth-30">
                <q-card-section>
                  <div class="center-items text-grey-2">
                    <img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0xf8E9F10c22840b613cdA05A0c5Fdb59A4d6cd7eF/logo.png" style="height: 30px; width:30px">
                    <div>DOE / Dogs Of Elon</div>
                  </div>
                </q-card-section>
              </q-card>
            </div>

            <q-card class="frm-input my-card text-white mint-tckn-card">
              <q-card-section>
                <div class="text-h6">Custom token detected</div>
              </q-card-section>
              <q-card-section class="q-pt-none">
                Did you know you can use our token factory to mint a safe token (ENMT) instead to get a free audit and boosted presale ranking?
              </q-card-section>
              <q-card-actions>
                <a href="javascript:;" class="crd-act-btn">
                  <span> Mint a safe token </span>
                </a>
              </q-card-actions>
            </q-card>

            <div class="prtcp-inpts frm-input">
              <div class="frm-input sm-inpt-left" style="min-width: 265px">
                <q-select dark filled v-model="partNetModel" :options="partNetOptions" label="Buyers participate with:" :options-html="true" />
              </div>
              <q-input disable readonly style="width: 100%" class="pair-inpt sm-inpt-right frm-input" outlined filled v-model="uniswapV2Pair" label="Uniswap V2 pair to be created:" dark/>
            </div>

            <q-card dark bordered class="info-card bg-grey-9 my-card">
              <q-card-section class="card-header">
                <div class="center-items">
                  <div class="text-subtitle3">Presale creator: </div> &nbsp;
                  <div class="text-subtitle1" style="font-weight: bold"> 0x0eBB...C0D1</div>
                </div>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <q-card-section card-body>
                This account will be the only account capable of adding presale information, editing presale contract paramaters and unlocking liquidity.
              </q-card-section>
            </q-card>
          </div>
          <div id="step-2" class="header-filler"></div>
          <div v-if="currentStep > 1 && isDesktop" class="step-header">
            <span class="step-text">Step 2</span>
          </div>
          <div v-show="currentStep > 2 && isDesktop || currentStep === 2" class="step-content q-pa-md">

            <q-input class="frm-input" dark type="number" filled outlined v-model.number="doeForPresale" label="How many DOE are up for presale?" hint="A minimum divisibility of 10,000 units (including decimals) is required for a presale.">
              <template v-slot:prepend>
                <div style="margin-top: 10px;">
                  <span class="text-white font-size-16">DOE: </span>
                </div>
              </template>
              <template v-slot:append>
                <div style="margin-top: 12px;">
                  <span style="font-size: 15px;">Balance: {{0}} </span>
                </div>
              </template>
            </q-input>
            <div class="space-20"></div>
            <div class="flex frm-input">
              <q-input class="sm-fixed-inpt-left" dark type="number" filled outlined v-model.number="softcap" label="Softcap"  hint="Must be > 0">
                <template v-slot:prepend>
                  <div style="margin-top: 10px;">
                    <span class="text-white font-size-16">DAI: </span>
                  </div>
                </template>
              </q-input>
              <q-input class="sm-fixed-inpt-right" dark type="number" filled outlined v-model.number="hardcap" label="Hardcap"  hint="Must be > 0" :rules="[val => !!val || 'Field is required']">
                <template v-slot:prepend>
                  <div style="margin-top: 10px;">
                    <span class="text-white font-size-16">DAI: </span>
                  </div>
                </template>
              </q-input>
            </div>

            <div class="prsl-inputs">
              <div class="liquidity-inpt frm-input sm-inpt-left">
                <div class="presale-section">
                  <div class="center-items">
                    <span class="text-white font-size-16">Presale rate</span>
                  </div>
                  <div class="center-items">
                    <span class="secondary-color" style="font-size: 19px;font-weight: bold;">1 DAI = 0 DOE</span>
                  </div>
                </div>
                <div class="listing-section" style="margin-top: 20px;margin-bottom: 35px;">
                  <div class="center-items">
                    <span class="text-white font-size-16">Listing rate</span>
                  </div>
                  <div class="center-items" style="margin-bottom: 10px;">
                    <span class="secondary-color" style="font-size: 19px;font-weight: bold;">1 DAI = 0 DOE</span>
                  </div>
                  <q-btn-toggle dark v-model="listingRate" class="listing-rate-inpt" no-caps spread rounded unelevated toggle-color="secondary" color="form-background-color" text-color="secondary"
                                :options="[{label: '0%', value: 0},{label: '10%', value: 10},{label: '15%', value: 15},{label: '25%', value: 25},{label: '30%', value: 30}]" />
                </div>
                <div class="liquidity-section">
                  <div class="center-items secondary-color">
                    <span style="font-weight: bold; font-size: 21px">{{ liquidityPercentage }}%</span>
                  </div>
                  <q-badge color="secondary">
                    <div style="padding:3px;font-size: 13px;">Percent of raised DAI used for liquidity</div>
                  </q-badge>
                  <q-slider v-model="liquidityPercentage" :min="60" :max="100" :step="1" label :label-value="liquidityPercentage + '%'" color="secondary" />
                </div >
                <div class="additional-tokens-section">
                  <div class="center-items">
                    <span class="text-white font-size-16">Additional tokens required for liquidity if hardcap is met</span>
                  </div>
                  <div class="center-items">
                  <span class="center-items" style="font-size: 19px;font-weight: bold;">
                    <img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0xf8E9F10c22840b613cdA05A0c5Fdb59A4d6cd7eF/logo.png" style="height: 30px; width:30px">
                     <span class="secondary-color" style="padding:0 5px; font-size:23px; font-weight: bolder;">{{0}}</span>
                     <span class="text-grey-2">DOE</span>
                  </span>
                  </div>
                </div>
              </div>

              <div class="presale-prediction-inpt frm-input sm-inpt-right">
                <div class="presale-section center-items" style="padding-bottom: 15px">
                  <span class="text-white" style="font-size: 18px;">Presale prediction</span>
                </div>
                <div class="liquidity-section">
                  <div class="center-items secondary-color">
                    <span style="font-weight: bold; font-size: 21px">{{ feeAndLiquidity }} DAI</span>
                  </div>
                  <q-badge color="secondary">
                    <div style="padding:3px;font-size: 13px;">Use the slider to predict fee and liquidity amounts <br> depending on amounts raised in presale</div>
                  </q-badge>
                  <q-slider flat v-model="feeAndLiquidity" :min="0" :max="100" :step="1" label :label-value="feeAndLiquidity + '%'" color="secondary" />
                </div >
                <div class="presale-info">
                  <div class="flex" style="justify-content: space-around;">
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="uncxFeeDai" label="Unicrypt fee" dark/>
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="daiLiquidity" label="DAI liquidity" dark/>
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="yourDai" label="Your DAI" dark/>
                  </div>
                  <div class="flex" style="justify-content: space-around;">
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="uncxFeeDoe" label="Unicrypt fee" dark/>
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="doeLiquidity" label="DOE liquidity" dark/>
                    <q-input disable readonly style="width: 30%;" class="pair-inpt frm-input" outlined filled v-model="yourDoe" label="Your DOE" dark/>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div id="step-3" class="header-filler"></div>
          <div v-if="currentStep > 2 && isDesktop" class="step-header">
            <span class="step-text">Step 3</span>
          </div>
          <div v-show="currentStep > 3 && isDesktop || currentStep === 3" class="step-content q-pa-md">
            <div class="allocation-section frm-input">
              <div class="allocation-impt">
                <div class="xsm-inpt-left wdth-70 frm-input">
                  <q-input dark type="number" filled outlined v-model.number="allocationPerUser" label="Max allocation per user"  hint="Must be above 0">
                    <template v-slot:prepend>
                      <div>
                        <span class="text-white font-size-16">DAI: </span>
                      </div>
                    </template>
                  </q-input>
                </div>
                <div class="xsm-inpt-right center-items wdth-30" style="margin-left: 20px;padding-bottom:30px;">
                  <q-btn style="height: 15px;min-width: 172px;" unelevated rounded no-caps color="secondary" label="Set allocation to 0 DAI" />
                  <div class="space-20"></div>
                </div>
              </div>
              <div class="center-items">
                <span class="text-white" style="padding-right: 5px;">0 unique participants &nbsp;</span>|
                <span class="secondary-color" style="font-weight: bold; padding-left: 5px;">&nbsp; Round 0 spots</span>
              </div>
              <div class="space-20"></div>
              <div class="center-items-evenly">
                <div class="text-subtitle1 text-white" style="font-weight: bold;">{{0}} UNCL spots</div> &nbsp;
                <div class="text-subtitle1 text-white" style="font-weight: bold;"> 0 whitelist spots</div>
              </div>
              <div class="center-items-evenly text-subtitle6 text-grey-5" style="font-size: 12px;">*These amounts change based on max allocation per user</div>
            </div>
            <q-select class="frm-input" dark filled v-model="partNetModel" :options="lockLiquidity" label="Lock Liquidity for:" :options-html="true" />

            <div class="custom-round-item frm-input">
              <div class="center-items xsm-inpt-left wdth-70" style="margin-bottom: 10px;">
                <span class="text-grey-4 font-size-16" style="font-weight: bold;">Would you like to set a custom round zero start date?</span>
              </div>
              <div class="center-items xsm-inpt-right wdth-30">
                <q-btn-toggle dark v-model="customRoundZeroSD" class="listing-rate-inpt" no-caps spread rounded unelevated toggle-color="secondary" color="form-background-color" text-color="secondary"
                              :options="[{label: 'No', value: false},{label: 'Yes', value: true}]" />
              </div>
            </div>

            <div class="presale-start-input frm-input">
                <q-input class="xsm-inpt-left wdth-70" style="padding-bottom: 50px" dark v-model="presaleStartDate" filled type="date" hint="Presale should ideally start 1 week from today to give you time to raise awareness"></q-input>
                <q-input class="xsm-inpt-right wdth-30" dark type="number" filled outlined v-model.number="presaleStartBlock" label="Block" ></q-input>
            </div>

            <div class="presale-end-input frm-input">
              <q-input class="xsm-inpt-left wdth-70" style="padding-bottom: 50px" dark v-model="presaleEndDate" filled type="date" hint="Presale duration: 0 seconds / 0 blocks"></q-input>
              <q-input class="xsm-inpt-right wdth-30" dark type="number" filled outlined v-model.number="presaleEndBlock" label="Block" ></q-input>
            </div>

            <q-card dark bordered class="error-card bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <div class="center-items">
                  <q-icon name="error" color="negative" size="2rem" />
                </div>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <q-card-section class="card-body">
                Your endblock is before the current block. Presale will instantly fail.
              </q-card-section>
            </q-card>

            <q-select class="frm-input" no-caps spread rounded dark v-model="selectCountryModel" :options="selectCountryOptions" label="Select your county" hint="Select your country of operation. Where is your business headquartered?"/>

            <div class="referral-address-item frm-input">
              <div class="center-items xsm-inpt-left wdth-70" style="margin-bottom: 10px;">
                <span class="text-grey-4 font-size-16" style="font-weight: bold;">Do you have a valid referral address?</span>
              </div>
              <div class="center-items xsm-inpt-right wdth-30">
                <q-btn-toggle dark v-model="referralAddress" class="listing-rate-inpt" no-caps spread rounded unelevated toggle-color="secondary" color="form-background-color" text-color="secondary"
                              :options="[{label: 'No', value: false},{label: 'Yes', value: true}]" />
              </div>
            </div>

            <div class="vest-participation-item frm-input">
              <div class="xsm-inpt-left wdth-70" style="margin-bottom: 10px;">
                <span class="center-items text-grey-4 font-size-16" style="font-weight: bold;">Do you plan to vest participants tokens?</span>
                <span class="center-items text-grey-6" style="font-size: 12px;">This feature must be requested from a Unicrypt admin, only they can implement it. However its important you set the flag to true so your users are aware in Round 0 that a percentage of their tokens will be vested post presale.</span>
              </div>
              <div class="center-items xsm-inpt-right wdth-30">
                <q-btn-toggle dark v-model="vestParticipationTokens" class="listing-rate-inpt" no-caps spread rounded unelevated toggle-color="secondary" color="form-background-color" text-color="secondary"
                              :options="[{label: 'No', value: false},{label: 'Yes', value: true}]" />
              </div>
            </div>
          </div>
          <div id="step-4" class="header-filler"></div>
          <div v-if="currentStep > 3 && isDesktop" class="step-header">
            <span class="step-text">Step 4</span>
          </div>
          <div v-show="currentStep > 4 && isDesktop || currentStep === 4" class="step-content q-pa-md">
            <q-card dark bordered class="info-card-2 bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <span class="center-items">Fees</span>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <div>
                <q-card-section class="card-body">
                  <div class="center-items">
                    <span class="left-item">Presale creation:</span>
                    <span class="right-item">0.2 ETH</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">2.2%:</span>
                    <span class="right-item">2.2% of raised DAI</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">2.2%:</span>
                    <span class="right-item">2.2% of sold DOE</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">Liquidity Lock:</span>
                    <span class="right-item">Standard locker fee</span>
                  </div>
                </q-card-section>
              </div>
            </q-card>

            <q-card dark bordered class="error-card bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <div class="center-items">
                  <q-icon name="error" color="negative" size="2rem" />
                </div>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <q-card-section class="card-body">
                Your tokenomics score is quite bad. Your presale stands a HIGH chance of failing due to the risk involved with so many tokens not in the presale or initial liquidity. Investors on Unicrypt are well trained on tokenomics. You should increase the amount of tokens for the presale and liquidity. Take this warning SERIOUSLY to prevent wasting time on an almost sure to fail presale.
              </q-card-section>
            </q-card>

            <q-card dark bordered class="info-card-2 bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <span class="center-items">Total DOE required</span>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <div>
                <q-card-section class="card-body">
                  <div class="center-items">
                    <span class="left-item">Presale:</span>
                    <span class="right-item">0</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">Amount for liquidity:</span>
                    <span class="right-item">0</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">Fees:</span>
                    <span class="right-item">0</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">Total Required:</span>
                    <span class="right-item">0</span>
                  </div>
                </q-card-section>
              </div>
            </q-card>

            <q-card dark bordered class="info-card-2 bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <span class="center-items">Whitelist and Hypemeter spots</span>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <div>
                <q-card-section class="card-body">
                  <div class="center-items">
                    <span class="left-item">UNCL spots:</span>
                    <span class="right-item">0</span>
                  </div>
                  <div class="center-items">
                    <span class="left-item">Whitelist spots:</span>
                    <span class="right-item">0</span>
                  </div>
                  <div class="center-items">
                    <span class="additional-ifno">If you would like more whitelist spots (20% of hardcap) please decrease the max allocation per user.</span>
                  </div>
                </q-card-section>
              </div>
            </q-card>

            <q-card dark bordered class="error-card bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <div class="center-items">
                  <q-icon name="error" color="negative" size="2rem" />
                </div>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <q-card-section class="card-body">
                Your tokenomics score is quite bad. Your presale stands a HIGH chance of failing due to the risk involved with so many tokens not in the presale or initial liquidity. Investors on Unicrypt are well trained on tokenomics. You should increase the amount of tokens for the presale and liquidity. Take this warning SERIOUSLY to prevent wasting time on an almost sure to fail presale.
              </q-card-section>
            </q-card>

            <q-card dark bordered class="error-card bg-grey-9 my-card frm-input">
              <q-card-section class="card-header">
                <div class="center-items">
                  <q-icon name="error" color="negative" size="2rem" />
                </div>
              </q-card-section>
              <q-separator inset class="info-card-separator" />
              <q-card-section class="card-body">
                You do not have enough ETH in your wallet to perform this transaction. 0.2 ETH required.
              </q-card-section>
            </q-card>

            <q-card dark bordered class="info-card-2 bg-grey-9 my-card frm-input">
              <div>
                <q-card-section class="card-body">
                  <div class="center-items">
                    <span class="additional-ifno">Please note: If the presale is a success, any unsold tokens are sent to the 0x00...dEaD burn address.</span>
                  </div>
                </q-card-section>
              </div>
            </q-card>
          </div>
          <div class="step-actions justify-evenly q-pa-md">
            <q-btn v-if="!isDesktop" v-show="currentStep !== 1" label="Back" color="secondary" class="step-btn" @click="backButtonClick"/>
            <q-btn label="Next" v-show="currentStep < 4" color="secondary" class="step-btn" @click="nextButtonClick"/>
          </div>

          <div v-if="currentStep === 4" class="confirm-actions justify-evenly q-pa-md">
            <q-btn size="17px" label="Approve" color="primary" class="submit-btn" @click="approveClick"/>
            <q-btn size="17px" label="Create Presale" color="primary" class="submit-btn" @click="createPresaleClick"/>
          </div>
        </q-form>

      </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'PresaleGeneratorForm',

  setup () {
    const $q = useQuasar()

    const presaleGeneratorForm = ref(null)
    const isDesktop = $q.platform.is.desktop;
    const currentStep = ref(1);
    const tokenAddres = ref(null)
    const uniswapV2Pair = ref('DAI / DOE')
    const doeForPresale = ref(null)
    const softcap = ref(null)
    const hardcap = ref(null)
    const listingRate = ref(10)
    const liquidityPercentage = ref(60)
    const feeAndLiquidity = ref(0)
    const uncxFeeDai = ref('0 DAI')
    const daiLiquidity = ref('0 DAI')
    const yourDai = ref('0 DAI')
    const uncxFeeDoe = ref('0 DOE')
    const doeLiquidity = ref('0 DOE')
    const yourDoe = ref('0 DOE')
    const allocationPerUser = ref('0.0')
    const customRoundZeroSD = ref(false)
    const presaleStartDate = ref(null)
    const presaleStartBlock = ref(0)
    const presaleEndDate = ref(null)
    const presaleEndBlock = ref(0)
    const referralAddress = ref(false)
    const vestParticipationTokens = ref(false)

    return {
      tokenAddres,
      currentStep,
      isDesktop,
      partNetModel: ref(null),
      partNetOptions: [
        {
          label: '<div style="align-items: center;display: flex;"><img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2/logo.png" class="mr-2" style="height: 26px; width: 26px;margin-right:10px"> Ethereum</div>',
          value: 'Ethereum',
          html: true
        },
        {
          label: '<div style="align-items: center;display: flex;"><img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0x6B175474E89094C44Da98b954EedeAC495271d0F/logo.png" class="mr-2" style="height: 26px; width: 26px;margin-right:10px"> DAI</div>',
          value: 'DAI',
          html: true
        },
        {
          label: '<div style="align-items: center;display: flex;"><img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599/logo.png" class="mr-2" style="height: 26px; width: 26px;margin-right:10px"> Wrapped BTC</div>',
          value: 'Wrapped BTC',
          html: true
        },
        {
          label: '<div style="align-items: center;display: flex;"><img src="https://raw.githubusercontent.com/trustwallet/assets/master/blockchains/ethereum/assets/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48/logo.png" class="mr-2" style="height: 26px; width: 26px;margin-right:10px"> USDC</div>',
          value: 'USDC',
          html: true
        }
      ],
      uniswapV2Pair,
      doeForPresale,
      softcap,
      hardcap,
      listingRate,
      liquidityPercentage,
      feeAndLiquidity,
      uncxFeeDai,
      daiLiquidity,
      yourDai,
      uncxFeeDoe,
      doeLiquidity,
      yourDoe,
      allocationPerUser,
      lockLiquidity: [
        {
          label: '1 month',
          value: '1'
        },
        {
          label: '2 month',
          value: '2'
        },
        {
          label: '3 month',
          value: '3'
        },
        {
          label: '6 month',
          value: '6'
        },
        {
          label: '12 month',
          value: '12'
        },
        {
          label: 'Max: 266 years',
          value: 'MAX'
        },
      ],
      customRoundZeroSD,
      presaleStartDate,
      presaleStartBlock,
      presaleEndDate,
      presaleEndBlock,
      selectCountryModel: ref(null),
      selectCountryOptions: [
        {
          label: 'France',
          value: 'FR'
        },
        {
          label: 'Ireland',
          value: 'IR'
        },
        {
          label: 'Italy',
          value: 'IT'
        },
        {
          label: 'United States',
          value: 'USA'
        },
      ],
      referralAddress,
      vestParticipationTokens,
      presaleGeneratorForm,

      backButtonClick () {
        currentStep.value = currentStep.value - 1;
      },

      nextButtonClick () {
        currentStep.value = currentStep.value + 1;
        setTimeout(() => {
          var element = document.getElementById('step-'+currentStep.value);
          if (isDesktop && currentStep.value <= 4 && element) {
            element.scrollIntoView({behavior: 'smooth', block: 'start', inline: 'nearest'});
          }
        }, 10);
      },

      approveClick () {
        // Do something
      },

      createPresaleClick () {
        // Do something
      },
    }
  }
});
</script>
