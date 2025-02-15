<template>
  <div class="flex">
    <div class="flex h-[6.25rem] w-[6.25rem]">
      <img
        :src="'/dashboard/images/integrations/' + integrationLogo"
        class="max-w-full p-6"
      />
    </div>
    <div class="flex flex-col justify-center m-0 mx-4 flex-1">
      <h3 class="text-xl text-slate-800 dark:text-slate-100">
        {{ integrationName }}
      </h3>
      <p>
        {{
          useInstallationName(
            integrationDescription,
            globalConfig.installationName
          )
        }}
      </p>
    </div>
    <div class="flex justify-center items-center mb-0 w-[15%]">
      <router-link
        :to="
          frontendURL(
            `accounts/${accountId}/settings/integrations/` + integrationId
          )
        "
      >
        <div v-if="integrationEnabled">
          <div v-if="integrationAction === 'disconnect'">
            <div @click="openDeletePopup()">
              <woot-submit-button
                :button-text="
                  $t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.BUTTON_TEXT')
                "
                icon-class="dismiss-circle"
                button-class="nice alert"
              />
            </div>
          </div>
          <div v-else>
            <button class="button nice">
              {{ $t('INTEGRATION_SETTINGS.WEBHOOK.CONFIGURE') }}
            </button>
          </div>
        </div>
      </router-link>
      <div v-if="!integrationEnabled">
        <a :href="integrationAction" class="button success nice">
          {{ $t('INTEGRATION_SETTINGS.CONNECT.BUTTON_TEXT') }}
        </a>
      </div>
    </div>
    <woot-delete-modal
      :show.sync="showDeleteConfirmationPopup"
      :on-close="closeDeletePopup"
      :on-confirm="confirmDeletion"
      :title="$t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.CONFIRM.TITLE')"
      :message="$t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.CONFIRM.MESSAGE')"
      :confirm-text="$t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.CONFIRM.YES')"
      :reject-text="$t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.CONFIRM.NO')"
    />
  </div>
</template>
<script>
import { mapGetters } from 'vuex';
import { frontendURL } from '../../../../helper/URLHelper';
import alertMixin from 'shared/mixins/alertMixin';
import globalConfigMixin from 'shared/mixins/globalConfigMixin';

export default {
  mixins: [alertMixin, globalConfigMixin],
  props: {
    integrationId: {
      type: [String, Number],
      required: true,
    },
    integrationLogo: { type: String, default: '' },
    integrationName: { type: String, default: '' },
    integrationDescription: { type: String, default: '' },
    integrationEnabled: { type: Boolean, default: false },
    integrationAction: { type: String, default: '' },
  },
  data() {
    return {
      showDeleteConfirmationPopup: false,
    };
  },
  computed: {
    ...mapGetters({
      currentUser: 'getCurrentUser',
      accountId: 'getCurrentAccountId',
      globalConfig: 'globalConfig/get',
    }),
  },
  methods: {
    frontendURL,
    openDeletePopup() {
      this.showDeleteConfirmationPopup = true;
    },
    closeDeletePopup() {
      this.showDeleteConfirmationPopup = false;
    },
    confirmDeletion() {
      this.closeDeletePopup();
      this.deleteIntegration(this.deleteIntegration);
      this.$router.push({ name: 'settings_integrations' });
    },
    async deleteIntegration() {
      try {
        await this.$store.dispatch(
          'integrations/deleteIntegration',
          this.integrationId
        );
        this.showAlert(
          this.$t('INTEGRATION_SETTINGS.DELETE.API.SUCCESS_MESSAGE')
        );
      } catch (error) {
        this.showAlert(
          this.$t('INTEGRATION_SETTINGS.WEBHOOK.DELETE.API.ERROR_MESSAGE')
        );
      }
    },
  },
};
</script>
