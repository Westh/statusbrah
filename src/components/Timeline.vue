<template>
  <div class='block'>
    <el-timeline>
      <el-timeline-item
        v-for='(item, index) in dates'
        :key='index'
        size='large'
        :timestamp='item.text'
        placement='top'
      >
        <el-card v-if='incidents.find(o => o.date === item.value)'>
          <h4>
            {{ incidents.find(o => o.date === item.value).title }}
          </h4>
          <div class='tagContainer'>
            <el-tag
              v-for='(item, index) in incidents.find(o => o.date === item.value).targets'
              :key='index'
              :type='getType(item.type)'
              effect='dark'
              class='tagSpacing'
            >
              {{ item.system }}
            </el-tag>
          </div>
          <p>
            {{ incidents.find(o => o.date === item.value).message }}
          </p>
          <div v-if='incidents.find(o => o.date === item.value).update'>
            <el-divider />
            <h4>
              Update.
            </h4>
            <p>
              {{ incidents.find(o => o.date === item.value).update.message }}
            </p>
          </div>
          <div v-if='incidents.find(o => o.date === item.value).partialResolve'>
            <el-divider />
            <h4>
              Partially resolved.
            </h4>
            <div class='tagContainer'>
              <el-tag
                v-for='(item, index) in incidents.find(o => o.date === item.value).partialResolve.targets'
                :key='index'
                :type='getType(item.type)'
                effect='dark'
                class='tagSpacing'
              >
                {{ item.system }}
              </el-tag>
            </div>
            <p>
              {{ incidents.find(o => o.date === item.value).partialResolve.message }}
            </p>
          </div>
          <div v-if='incidents.find(o => o.date === item.value).resolve'>
            <el-divider />
            <h4>
              Resolved.
            </h4>
            <p>
              {{ incidents.find(o => o.date === item.value).resolve.message }}
            </p>
          </div>
        </el-card>
        <h3 v-else>
          No incidents reported.
        </h3>
      </el-timeline-item>
    </el-timeline>
  </div>
</template>

<script>
export default {
  name: 'Timeline',
  props: ['incidents'],
  data () {
    return {
      dates: []
    }
  },
  methods: {
    getLastWeek () {
      const date = new Date()

      const options = { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' }

      for (let i = 0; i < 7; i++) {
        const tempDate = new Date()
        tempDate.setDate(date.getDate() - i)
        const strText = tempDate.toLocaleDateString('en-AU', options)
        const strValue = tempDate.toISOString().split('T')[0]
        this.dates.push({ text: strText, value: strValue })
      }
    },
    getType (type) {
      if (type === 'error') {
        return 'danger'
      } else if (type === 'maintenance') {
        return ''
      } else {
        return type
      }
    }
  },
  mounted () {
    this.getLastWeek()
  }
}
</script>

<style scoped>
>>>.el-timeline-item__timestamp {
    color: #909399;
    line-height: 1;
    font-size: 17px;
}
>>>.el-timeline-item__timestamp.is-top {
    margin-bottom: 8px;
    padding-top: 2px;
}
@media screen and (min-width: 600px) {
  .tagContainer {
    display: inline-block;
    float: right;
  }
  .tagSpacing {
    margin-right: 4px;
  }
}
@media screen and (max-width: 600px) {
  .tagContainer {
    display: block;
  }
  .tagSpacing {
    margin-right: 4px;
    margin-top: 4px;
  }
}
.el-card {
  margin-top: 15px;
}
h4 {
  display: inline-block;
  margin-top: 6px;
}
h3 {
  color: #6a737d
}
</style>
