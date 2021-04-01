<template>
<div style="background-color: #ebecef;" class="wrapper">
   <div class="row" style="margin-right:0">
      <div class="col-md-6">
         <div class="header" style="margin-bottom: 1%; margin-left:15px ">
            <h4>EVENTS</h4>
         </div>
      </div>
      <div class="col-md-6">
         <div class="header" style="margin-bottom: 1%; margin-left:15px; text-align: center; ">
            <h4>EVENT DETAILS</h4>
         </div>
      </div>
   </div>
   <div class="row" style="margin-right: 0; box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);">
      <div class="col-md-8" style="margin-right: 20px;">
         <div class="row eventsList" v-for="data in example_response.data" :key="data.id">
            <div v-if="selected != null" class="col-md-12 tableRow mt-1" style="cursor: pointer;" :class="{ 'leftBorder' : data.details[4].value === '-','tableActive': selected.id == data.id}" @click="selected = data">
               <div class="row">
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[0].title}}</label>
                     <p class="tabDesc">{{new Date(data.details[0].value).toLocaleString()}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[1].title}}</label>
                     <p class="tabDesc">{{data.details[1].value}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">ID</label>
                     <p class="tabDesc">{{data.id}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[3].title}}</label>
                     <p class="tabDesc">{{data.details[3].value}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[4].title}}</label>
                     <p class="tabDesc">{{data.details[4].value}}</p>
                  </div>
               </div>
            </div>
            <div v-else class="col-md-12 tableRow mt-1" style="cursor: pointer;" :class="{ 'leftBorder' : data.details[4].value === '-'}" @click="selected = data">
               <div class="row">
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[0].title}}</label>
                     <p class="tabDesc">{{new Date(data.details[0].value).toLocaleString()}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[1].title}}</label>
                     <p class="tabDesc">{{data.details[1].value}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">ID</label>
                     <p class="tabDesc">{{data.id}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[3].title}}</label>
                     <p class="tabDesc">{{data.details[3].value}}</p>
                  </div>
                  <div class="col-md-2">
                     <label class="tabLabel">{{data.details[4].title}}</label>
                     <p class="tabDesc">{{data.details[4].value}}</p>
                  </div>
               </div>
            </div>
         </div>
      </div>
      <div class="col-evreka-4 rightBar">
         <div class="eventCard">
            <button v-if="selected != null" type="button" style="width: 190px;" @click="selected.details[4].value = '-'" class="btnFalse">NO ACTION NEEDED</button>
            <button v-else type="button" style="width: 190px;" @click="selected.details[4].value = '-'" class="btnFalse">NO ACTION NEEDED</button>
         </div>
         &nbsp;
         <div class="eventCard">
            <b-modal id="modal-lg-action" size="lg" title="Large Modal" hide-footer centered hide-header >
               <b-tabs v-model="tabIndex" content-class="mt-3" v-if="selected != null" align='center' >
                  <b-tab title="➊ SELECT ACTION" active >
                     <div @click="radioAction=true" class="selectActionButton" :class="{'actionActive' :radioAction == true}">
                        <h4 style="">Mark as Resolved</h4>
                        <p>Mark this event as resolved and enter the details of the resolution.</p>
                     </div>
                     <div @click="radioAction=false" class="selectActionButton"  :class="{'actionActive' :radioAction == false}">
                        <h4>Change Asset</h4>
                        <p style="padding-bottom: 10px;">Change the asset with another one.</p>
                     </div>
                     <div style="position: relative; flex: 1 1 auto; padding: 1rem;">
                        <button style="margin-left: 285px;" type="button" class="btnTrue nextBtn" @click="tabIndex++">NEXT</button>
                     </div>
                  </b-tab>
                  <b-tab title="➋ TAKE ACTION">
                     <h4 v-if="radioAction" style="font-size:1.3rem !important;">Mark as Resolved</h4>
                     <p   v-if="radioAction" style="padding-bottom: 10px; font-size:15px !important;">Mark this event as resolved and enter the details of the resolution.</p>
                     <h4 v-if="!radioAction" style="font-size:1.3rem !important;">Change Asset</h4>
                     <p   v-if="!radioAction" style="padding-bottom: 10px; font-size:15px !important;">Mark this event as resolved and enter the details of the resolution.</p>
                     <div>
                        <p style="margin-bottom: 0; font-weight:500;">Resolution Detail*</p>
                        <b-form-textarea
                           id="textarea"
                           v-model="selected.details[4].value"
                           placeholder="Enter resolution detail..."
                           rows="3"
                           max-characters='300'
                           max-rows="6"
                           ></b-form-textarea>
                     </div>
                     <div style="position: relative; flex: 1 1 auto; padding: 1rem;">
                        <button style="margin-left: 211px;" type="button" class="btnTrue backBtn" @click="tabIndex--">BACK</button>
                        <button  type="button" class="btnTrue nextBtn"  @click="$bvModal.hide('modal-lg-action'),showAlert()">TAKE ACTION</button>
                     </div>
                  </b-tab>
               </b-tabs>
            </b-modal>
            <button style="width: 190px;" type="button" @click="$bvModal.show('modal-lg-action')" class="btnTrue">TAKE ACTION</button>
         </div>
         <b-tabs content-class="mt-3" style="margin-top: 20px; letter-spacing: 1px;" v-if="selected != null">
            <b-tab title="DETAILS" active>
               <div class="row" style="letter-spacing: 0px;">
                  <div class="col-md-6 text-left ln-1 mt-3" v-for="(sel,name) in selected.details" :key="name">
                     <label class="tabLabel">{{sel.title}}</label>
                     <br>
                     <label class="tabDesc rightTextDesc">{{sel.value}}</label>
                  </div>
               </div>
            </b-tab>
            <b-tab title="LOCATION" v-if="selected.location != null">
               <p style="display:none;">{{selected_location = "https://maps.google.com/maps?q="+selected.location.latitude+", "+selected.location.longitude+"&z=15&output=embed"}}</p>
               <iframe v-bind:src="selected_location" width="100%" height="270" frameborder="0" style="border:0"></iframe>
            </b-tab>
            <b-tab title="MEDIA" v-if="selected.media[0].url != null">
               <img class="imgCover" v-b-modal.modal-lg v-if="selected.media[0].type == 'image'" v-bind:src="selected.media[0].url" @click="$bvModal.show('modal-lg')" style="width:100%;cursor:pointer;">
               <audio controls v-if="selected.media[0].type == 'audio'">
                  <source v-bind:src="selected.media[0].url"/>
               </audio>
               <b-modal v-if="selected.media[0].type == 'image'" id="modal-lg" size="lg" hide-footer hide-header>
                  <div class="d-block text-center">
                     <img  v-bind:src="selected.media[0].url" style="width:100%;">
                  </div>
               </b-modal>
            </b-tab>
         </b-tabs>
      </div>
   </div>
</div>
</template>

<script>

import $ from 'jquery';
export default {

    data(){
        return{
          actionSelect:true,
            radioAction:true,
            tabIndex:1,
            selected_location: "",
            selected: null,
            example_response: {
  last_update_time: 1597384772000,
  timezone: 'Europe/Istanbul',
  action_descriptions: {
    '0': '-',
    '1': 'Bu olayı çözüldü olarak bildir ve çözümü not et.',
    '2': 'Bu olay için plansız bir görev oluştur.',
    '3': 'Bu varlığı son görülen konumuna taşı.',
  },
  success: true,
  type_detail: {
    observation: {
      '0': 'Gözlem Bildirimi',
      '1': 'Yapılamayan Görev',
    },
    vcr: '',
    immediate_alerts: {
      '0': 'Hız',
      '1': 'Rölanti',
      '2': 'Mola',
      '3': 'Pasif Test',
      '4': 'Tablet Şarj Olmuyor',
      '5': 'GV300 Pil Alarmı',
    },
    incident: '',
    cleancity: '',
    asset: {
      '0': 'Sıcaklık Artışı',
      '1': 'Kritik Seviye',
      '2': 'Dolu Konteyner',
      '3': 'Yer Değişimi',
      '4': 'Alan Dışı Toplama',
      '5': 'Toplanma',
    },
    performance: {
      '0': 'Geç Açılma',
      '1': 'Eksik Ziyaret',
      '2': 'Uzakta Kapatma',
      '3': 'Rota Açılmaması',
      '4': 'Uzakta Açılma',
      '5': 'Bölge Dışı Konteyner Toplama',
      '6': 'Araç Değişikliği',
      '7': 'Şoför Değişikliği',
      '8': 'Plansız Rota',
      '9': 'Şarjdan Kapanma',
      '10': 'Hiç Görev Tamamlanmadı',
      '11': 'Görev Tamamlanamayacak',
    },
  },
  msg: '',
  data: [
    {
      media: [
        {
          url: 'https://media.evreka.co/1588172625.mp3',
          type: 'audio',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-04-29T15:03:44.871Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 10:51',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'dsfvbnv ',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13409,
      location: {
        latitude: 39.9229025,
        type: 'single_point',
        longitude: 32.8419197,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1588172618.mp3',
          type: 'audio',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-04-29T15:03:37.406Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 10:51',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'dsfvbnv ',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13408,
      location: {
        latitude: 39.9229025,
        type: 'single_point',
        longitude: 32.8419197,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1588172548.mp3',
          type: 'audio',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-04-29T15:02:27.652Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 10:51',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'dsfvbnv ',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13407,
      location: {
        latitude: 39.9229357,
        type: 'single_point',
        longitude: 32.8419088,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1588172539.mp3',
          type: 'audio',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-04-29T15:02:19.094Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 10:51',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'dsfvbnv ',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13406,
      location: {
        latitude: 39.9228845,
        type: 'single_point',
        longitude: 32.8419053,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1587840179.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '2019',
      },
      details: [
        {
          format: 'date',
          value: '2020-04-25T18:42:58.885Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/67',
          value: 'v6.5.2.7 test1',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Asım Doğan NAMLI',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13397,
      location: {
        latitude: 39.8959337,
        type: 'single_point',
        longitude: 32.8335157,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1584440476.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-03-17T10:21:15.009Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 13:04',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Flat Tire',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13003,
      location: {
        latitude: 39.9229594,
        type: 'single_point',
        longitude: 32.8418861,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1584439637.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '03085',
      },
      details: [
        {
          format: 'date',
          value: '2020-03-17T10:07:15.719Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Plansız Rota 13:04',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'İmren KARALAR',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 13002,
      location: {
        latitude: 39.922966,
        type: 'single_point',
        longitude: 32.8418967,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1584021116.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '2019',
      },
      details: [
        {
          format: 'date',
          value: '2020-03-12T13:51:55.895Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/67',
          value: 'v6.5.2.3 test',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Asım Doğan NAMLI',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12982,
      location: {
        latitude: 39.8921736,
        type: 'single_point',
        longitude: 32.7818015,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580124682.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-27T11:37:50.009Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 920,
        driver_id: '1234',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T11:31:19.692Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/67',
          value: 'Ad Hoc Route 12:41',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Flat Tire',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '00000',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Berkay AKÇORA',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12834,
      location: {
        latitude: 39.8974902,
        type: 'single_point',
        longitude: 32.7761183,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580122489.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: 'deneme',
          task_id: null,
          title: 'Çözüm Bildir',
          user: 'evreka',
          date: '2020-01-27T10:58:46.343Z',
          action_taken: true,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '123213',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T10:54:48.945Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Ad Hoc Route 12:43',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Çözüm Bildir',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Mehmet PANCAROĞLU',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12833,
      location: {
        latitude: 39.8974793,
        type: 'single_point',
        longitude: 32.7762278,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580120425.mp3',
          type: 'audio',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '123213',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T10:20:25.340Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Ad Hoc Route 12:43',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: '-',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Mehmet PANCAROĞLU',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12832,
      location: {
        latitude: 39.8975212,
        type: 'single_point',
        longitude: 32.7762372,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580120208.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-27T10:17:06.304Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '123213',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T10:16:48.601Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Ad Hoc Route 12:43',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Mehmet PANCAROĞLU',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12831,
      location: {
        latitude: 39.8974983,
        type: 'single_point',
        longitude: 32.7762436,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580119903.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment:
            'sgsfkgskgg\ngfgfgdfg\ndsmgsgnsmgnsmgnsmgnsmnsmgnsmgnsmgsgmngsd fmgLorem ipsum dolor sit amet, consectetur adipiscing elit. Duis volutpat arcu et lacus lobortis, sit amet auctor velit rhoncus. Nam eu ipsum ante. Proin imperdiet et eros sit amet cursus. Sed s',
          task_id: null,
          title: 'Çözüm Bildir',
          user: 'evreka',
          date: '2020-01-27T10:13:19.855Z',
          action_taken: true,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 156,
        driver_id: '123213',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T10:11:43.221Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: 'Ad Hoc Route 12:43',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Flat Tire',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Çözüm Bildir',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '06 ADN 61',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Mehmet PANCAROĞLU',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12830,
      location: {
        latitude: 39.8975006,
        type: 'single_point',
        longitude: 32.77625,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1580119225.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-27T10:08:50.157Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 920,
        driver_id: '1234',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-27T10:00:25.582Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/67',
          value: 'Ad Hoc Route 12:41',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: '00000',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'Berkay AKÇORA',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12828,
      location: {
        latitude: 39.8974943,
        type: 'single_point',
        longitude: 32.7762561,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667909.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-14T06:17:10.629Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:51:47.014Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12805,
      location: {
        latitude: 39.8974852,
        type: 'single_point',
        longitude: 32.7762242,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667889.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-14T06:17:13.864Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:51:28.500Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12804,
      location: {
        latitude: 39.8974852,
        type: 'single_point',
        longitude: 32.7762242,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667792.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: '',
          title: 'Aksiyon Gerekmiyor',
          user: '',
          date: '',
          action_taken: false,
          action_id: 0,
        },
        {
          comment: 'jhghgjh',
          task_id: null,
          title: 'Çözüm Bildir',
          user: 'evreka',
          date: '2020-01-14T06:17:23.206Z',
          action_taken: true,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:49:52.321Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Çözüm Bildir',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12803,
      location: {
        latitude: 39.8974827,
        type: 'single_point',
        longitude: 32.7762213,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667749.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-14T06:17:33.243Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:49:08.676Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'truck renkli',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'truck',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12802,
      location: {
        latitude: 39.8974827,
        type: 'single_point',
        longitude: 32.7762213,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667731.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-14T06:17:40.860Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:48:50.933Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Flat Tire',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12801,
      location: {
        latitude: 39.8974834,
        type: 'single_point',
        longitude: 32.7762185,
      },
    },
    {
      media: [
        {
          url: 'https://media.evreka.co/1578667657.jpg',
          type: 'image',
        },
      ],
      actions: [
        {
          comment: '',
          task_id: null,
          title: 'Aksiyon Gerekmiyor',
          user: 'evreka',
          date: '2020-01-10T14:48:17.103Z',
          action_taken: true,
          action_id: 0,
        },
        {
          comment: '',
          task_id: '',
          title: 'Çözüm Bildir',
          user: '',
          date: '',
          action_taken: false,
          action_id: 1,
        },
      ],
      extras: {
        new: false,
        vehicle_id: 683,
        driver_id: '95',
      },
      details: [
        {
          format: 'date',
          value: '2020-01-10T14:47:35.914Z',
          title: 'Tarih',
        },
        {
          format: 'incident_type',
          value: 'Breakdown',
          title: 'Tip',
        },
        {
          format: 'string',
          link: '/routing/operations/1',
          value: '122',
          title: 'Rota İsmi',
        },
        {
          format: 'string',
          value: 'Engine Malfunction',
          title: 'Kategori',
        },
        {
          format: 'string',
          value: 'Aksiyon Gerekmiyor',
          title: 'Aksiyon',
        },
        {
          format: 'string',
          value: '-',
          title: 'Kalan Görev(ler)',
        },
        {
          format: 'vehicle',
          value: 'imren',
          title: 'Araç',
        },
        {
          format: 'string',
          value: 'ozde12',
          title: 'Şoför',
        },
      ],
      type: 'incident',
      id: 12800,
      location: {
        latitude: 39.8974809,
        type: 'single_point',
        longitude: 32.7762164,
      },
    },
  ],
  type: 'incident',
            },
            
        }
    },
  methods: {
    showAlert() {
      // Use sweetalert2
      this.$swal({
  icon: 'success',
  title: 'ACTION HAS BEEN TAKEN!',
  text: "You can see the action details from details tab.",
  showConfirmButton: false,
  timer: 2000
});
      
    },
  },  
  mounted(){

     this.selected= this.example_response.data[0],
    $('body').on('click','.tableRow',function(){
       // selected = 99;
        $(".tableRow").removeClass("tableActive");
        $(this).addClass('tableActive');
    });
  },


 
}

</script>

