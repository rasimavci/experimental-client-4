
<template>
            <div class="button" v-show="item === 'VIDEO'">
              <x-button @click.native="prev" type="primary" @click="makeCall(true)">Video Call {{activeCall.calleeName}}</x-button>
            </div>



                  <div v-show="item === 'VOICE'" class="tabCenter" >
                    <div class="call-button-container" @click="makeCall(false)">
                            <div class="call-button-icon">
                                <i class="icon icon-start_call_outline_white"></i>
                            </div>
                            <div class="call-button-text" v-if = "activeCall.state === 'ENDED' || activeCall.state === '' || !activeCall.state" >Call {{activeCall.to}}</div>
                            <div class="call-button-text" v-if = "activeCall.state === 'RINGING' || activeCall.state === 'INITIALIZING' " >Calling {{activeCall.to}}</div>
                        </div>
                    </div>

</template>

<script>
// bunun yerine sadece div konuldu ve if denildi..
</script>
