<template>
  <div>
    <CardRecentEvent 
      :header="`🎟 ${addCommas(record.blockNumber)}-${record.index} - ${record.record.event.section}.${record.record.event.method}`"
      :content="`${record.record.event.meta.documentation[0]}`"
      :event="processedData"
    />
  </div>
</template>
<script lang="ts" >
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import CardRecentEvent from '@/components/shared/CardRecentEvent.vue'
import Connector from '@vue-polkadot/vue-api';

@Component({
  components: {
    CardRecentEvent
  }
})
export default class SingleEventDetail extends Vue {
  private phase: any = '';
  private sanitizedEvent: any = '';
  private processedData: any[] = [];
  @Prop() private record!: any;

  private addCommas(num: any) {
     return num.toString().replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ',');
  }

  private async mounted(): Promise<void> {
    this.sanitizedEvent = this.record && this.record
    // this.processRecord(this.record)
  }

  @Watch('record')
  private async processRecord(record: any) {    
    // console.log('SingleEventDetail -> processRecord -> record', record.record);
    // console.log('SingleEventDetail -> processRecord -> X', record.record.event.typeDef);
    const types = record.record.event.typeDef
    record.record.event.data.forEach((data: any, index: any) => {
      // console.log('SingleEventDetail -> processRecord -> types[index].type', types[index].type);
      // this.processedData.push(types[index].type, data.toString())
      // const d = {'k': types[index].type, 'i': data.toString()}
      this.processedData.push([types[index].type, data])
      // console.log('SingleEventDetail -> processRecord -> data.toString()', data.toString());
    });
    // console.log('SingleEventDetail -> processRecord -> record.record.data.toString()', record.record.data);
  }
}
</script>
