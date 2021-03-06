<template>
  <div>
    <div class="mb-4 d-flex">
      <refresh-button :loading="loading" @refresh="refresh" v-show="isRefreshed" />
      <template v-if="groupActions">
        <actions :options="groupActions" button-type="default" group />
      </template>
    </div>
    <card-list v-if="hasData" :list="list.data" :cardFields="cardFields" :singleActions="singleActions" />
    <loader v-else :loading="loading" />
    <a-pagination
      v-show="showPageer && list.total !== 0"
      class="my-3 text-center"
      showSizeChanger
      :pageSize.sync="list.limit"
      :total="list.total"
      @showSizeChange="sizeChange"
      v-model="currentPage" />
  </div>
</template>

<script>
import * as R from 'ramda'
import CardList from './CardList'
import Loader from './Loader'
import Actions from '@/components/PageList/Actions'
import RefreshButton from '@/components/PageList/RefreshButton'

export default {
  name: 'PageCardList',
  components: {
    Actions,
    RefreshButton,
    CardList,
    Loader,
  },
  props: {
    list: {
      type: Object,
      required: true,
    },
    singleActions: {
      type: Array,
    },
    groupActions: {
      type: Array,
    },
    cardFields: {
      type: Object,
      required: true,
    },
    showPageer: {
      type: Boolean,
      default: true,
    },
    isRefreshed: {
      type: Boolean,
      default: true,
    },
  },
  data () {
    return {
      currentPage: 1,
    }
  },
  computed: {
    loading () {
      return this.list.loading
    },
    hasData () {
      return !R.isEmpty(this.list.data) && !R.isNil(this.list.data)
    },
  },
  watch: {
    currentPage (val) {
      this.list.offset = (val - 1) * this.list.limit
      this.refresh()
    },
  },
  methods: {
    refresh () {
      this.list.refresh()
    },
    sizeChange (current, limit) {
      this.list.offset = (current - 1) * this.list.limit
      this.list.limit = limit
      this.refresh()
    },
  },
}
</script>
