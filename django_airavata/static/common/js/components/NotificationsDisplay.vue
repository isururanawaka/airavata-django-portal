<template>
  <div id="notifications-display">
    <transition-group name="fade" tag="div">
      <b-alert v-for="unhandledError in unhandledErrors" variant="danger" :key="unhandledError.id" show dismissible @dismissed="dismissUnhandledError(unhandledError)">
        {{ unhandledError.message }}
      </b-alert>
      <b-alert v-for="notification in notifications" :variant="variant(notification)" :key="notification.id" :show="notification.duration > 0 ? notification.duration : true"
        dismissible @dismissed="dismissNotification(notification)">
        {{ notification.message }}
      </b-alert>
    </transition-group>
  </div>
</template>

<script>
import { errors } from "django-airavata-api";
import NotificationList from "../notifications/NotificationList";

export default {
  name: "notifications-display",
  data() {
    return {
      notifications: NotificationList.list,
      unhandledErrors: errors.UnhandledErrorDisplayList.list
    };
  },
  methods: {
    dismissNotification: function(notification) {
      NotificationList.remove(notification);
    },
    dismissUnhandledError: function(unhandledError) {
      errors.UnhandledErrorDisplayList.remove(unhandledError);
    },
    variant: function(notification) {
      if (notification.type === "SUCCESS") {
        return "success";
      } else if (notification.type === "ERROR") {
        return "danger";
      } else if (notification.type === "WARNING") {
        return "warning";
      } else {
        return "secondary";
      }
    }
  }
};
</script>

<style>
#notifications-display {
  position: fixed;
  top: 75px;
  left: 20vw;
  width: 60vw;
  z-index: 10000;
}
</style>


