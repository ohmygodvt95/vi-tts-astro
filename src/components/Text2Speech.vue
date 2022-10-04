<template>
    <div>
        <textarea class="form-control" rows="10" v-model="text" placeholder="Input something great..." maxlength="1024"></textarea>
        <hr>
        <div class="d-flex justify-content-between align-items-center">
            <div class="" style="width: 100%; min-height: 20px; padding-right: 25px;">
                <audio v-if="audioUrl" controls style="width: 100%">
                    <source :src="audioUrl" type="audio/wav">
                    Your browser does not support the audio element.
                </audio>
            </div>
            <div style="width: 200px;">
                <button class="btn btn-primary btn-lg" style="width: 200px;"  @click="submit" :disabled="isLoading">
                    <span v-if="!isLoading">Submit</span>
                    <div v-else class="text-center">
                        <div class="spinner-border" role="status">
                            <span class="visually-hidden">Loading...</span>
                        </div>
                    </div>
                </button>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import axios from 'axios';

const text = ref('');
const audioUrl = ref();
const isLoading = ref(false);
const submit = () => {
    isLoading.value = true;
    audioUrl.value = null;
    axios.post('https://api.supervisor.live/text2speech', {
        text: text.value,
        speaker: 0
    }, {
        responseType: 'blob',
    }).then(res => {
        audioUrl.value = URL.createObjectURL(new Blob([res.data]))
    }).catch(() => {
        alert('Sorry! We can\'t process current input.');
    })
    .finally(() => {
        isLoading.value = false;
    });
};
</script>
