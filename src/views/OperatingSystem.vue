<script lang="ts">
import Vue, { PropType } from 'vue';

import { getLatestPackageOperatingSystemToRevision, OperatingSystemToRevisionMapping } from '@/lib/api/extension.service';

const AppId = process.env.VUE_APP_APP_ID as string;

export default Vue.extend({
  props: {
    category: {
      type: String as PropType<string>,
      required: true,
    },
  },

  data() {
    return {
      loading: false,
      revisionByOperatingSystems: {
        linux: 'unknown',
        macosx: 'unknown',
        win: 'unknown',
      } as OperatingSystemToRevisionMapping,
    };
  },

  methods: {
    async setRevisionByOperatingSystems() {
      getLatestPackageOperatingSystemToRevision({
        appId: AppId,
      }).then((res) => {
        this.revisionByOperatingSystems = res;
      });
    },
  },

  activated() {
    this.setRevisionByOperatingSystems();
  },

});

</script>

<template>
  <v-container
    fluid
  >
    <div class="text-center">
      <v-btn
        class="ma-2"
        :loading="loading"
        :disabled="loading"
        color="success"
      >
        Linux
        <template v-slot:loader>
          <span>Loading...</span>
        </template>
      </v-btn>
    </div>
    <v-row>
      <v-col>
        {{ category }}/{{revisionByOperatingSystems.linux }}/linux
      </v-col>
      <v-col>
        {{ category }}/{{revisionByOperatingSystems.macosx }}/macosx
      </v-col>
      <v-col>
        {{ category }}/{{revisionByOperatingSystems.win }}/win
      </v-col>
    </v-row>
  </v-container>
</template>
