<template>
    <div class="d-flex">
    <div  v-for="(item, itemIdx) in items" :key="itemIdx">
<div class="mx-4">
    {{ item.title }}
</div>
<div v-for="(card, cardIdx) in item.cards" :key="cardIdx">
    <v-card class="ma-4 pa-1" color="#E3F2FD">
        <div>ID cделки: {{ card.OFFER_ID }}</div>
        <div>ФИО: {{ card.CONTACT_TITLE }}</div> 
    <div>Тип сделки: {{ card.OFFER_TYPE_ID ? card.OFFER_TYPE_ID : 'Unknow' }}</div>
    <div>Cегмент сделки: {{ card.SEGMENT_NAME }}</div>
    
    </v-card>
</div>
    </div></div>
</template>
<script>    
const API_GET_DEALS = 'https://nastintesthodl.stocrm.ru/api/external/v1/offers/get_from_filter?SID=10813_0c0a9a2f86eab09196705a274378b64a&SORT[DATE_CREATE]=ASC'
const API_GET_DEALS_STATUS = 'https://nastintesthodl.stocrm.ru/api/external/v1/offer/all_statuses?SID=10813_0c0a9a2f86eab09196705a274378b64a&BOARD_ID=1843'
export default {
    data(){
        return {
            deals : [],
            items : [{}]
        }
    },
    async created(){
        const rawDealStatus = await fetch(API_GET_DEALS_STATUS)
        const { RESPONSE } = await rawDealStatus.json()
        const rawDeals = await fetch(API_GET_DEALS)
        this.deals = await rawDeals.json()
        console.log(this.deals)
        for (let key in RESPONSE){
            const title = RESPONSE[key]['TITLE']
            const cards = this.deals.RESPONSE.DATA.filter((item)=>item['STATUS_NAME'] === title)
            this.items.push({title, cards})
        }
    }
}
</script>