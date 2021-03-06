<template>
  <div class="m-d-img media-info-file">
    <!-- for folder -->
    <div class="folder" v-if="folder">
      <v-icon size="150">folder</v-icon>
    </div>

    <!-- for image -->
    <v-img
      class="m-gradient"
      aspect-ratio="1.75"
      :src="item.imgUrl"
      v-else-if="image || item.extension == 'pdf'"
      :lazy-src="item.imgLazyUrl"
    ></v-img>

    <!-- for audio -->
    <div class="video" v-else-if="video">
      <video class="media-video-player" controls controlslist="nodownload">
        <source :src="item.filePath" type="video/mp4">
      </video>
    </div>

    <!-- for audio -->
    <div class="audio" v-else-if="audio">
      <audio class="media-audio-player" controls controlslist="nodownload">
        <source :src="item.filePath" type="audio/mpeg">
      </audio>
    </div>

    <!-- for other files -->
    <div class="file" v-else>
      <v-img aspect-ratio="1.75" contain :src="item.extImg" :lazy-src="item.extImg"></v-img>
    </div>

    <br>
    <div class="m-d-details">
      <h3 class="headline mb-0">
        <strong>{{item.name}}</strong>
      </h3>
    </div>

    <div class="m-d-info">
      <table>
        <tr>
          <td class="m-d-td">Type</td>
          <td class="m-d-td">
            <strong>{{ item.type }}</strong>
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Modified Date</td>
          <td class="m-d-td">
            <strong>{{ getTime(item.modified_date) }}</strong>
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Size</td>
          <td class="m-d-td">
            <strong>{{ getSize(item.size) }}</strong>
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Extension</td>
          <td class="m-d-td">
            <strong>{{ item.extension }}</strong>&nbsp;
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Mime Type</td>
          <td class="m-d-td">
            <strong>{{ item.mime_type }}</strong>
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Dimensions</td>
          <td class="m-d-td">
            <strong>{{ item.height }} x {{ item.width }}</strong>
          </td>
        </tr>
        <tr>
          <td class="m-d-td">Owner</td>
          <td class="m-d-td">
            <strong>{{ item.owner }}</strong>
          </td>
        </tr>
      </table>
    </div>

    <v-btn flat color="success">Copy Details</v-btn>
    <v-btn flat color="success">Share</v-btn>
    <br>
    <div class="media-toolbar"></div>
    <br>
  </div>
</template>

<script>
import { api } from "./../../../app/Api";

export default {
  name: "media-info-file",
  data() {
    return {
      videoExt: ["mp4"],
      audioExt: ["mp3", "webm"],
      fileExt: ["zip", "ico", "pdf"],
      imageExt: ["jpg", "jpeg", "png", "gif", "tif"],
      iconsMap: {
        mp3: "library_music",
        zip: "archive",
        mp4: "music_video",
        webm: "library_music",
        default: "insert_drive_file"
      }
    };
  },
  props: ["item"],
  computed: {
    video: function() {
      if (this.videoExt.indexOf(this.item.extension.toLowerCase()) != -1) {
        return true;
      } else {
        return false;
      }
    },
    audio: function() {
      if (this.audioExt.indexOf(this.item.extension.toLowerCase()) != -1) {
        return true;
      } else {
        return false;
      }
    },
    image: function() {
      if (this.imageExt.indexOf(this.item.extension.toLowerCase()) != -1) {
        return true;
      } else {
        return false;
      }
    },
    file: function() {
      if (this.fileExt.indexOf(this.item.extension.toLowerCase()) != -1) {
        return true;
      } else {
        return false;
      }
    },
    folder: function() {
      if (this.item.type == "dir") {
        return true;
      } else {
        return false;
      }
    },
    icon: function() {
      if (
        this.iconsMap[this.item.extension.toLowerCase()] &&
        this.iconsMap[this.item.extension.toLowerCase()] != ""
      ) {
        return this.iconsMap[this.item.extension.toLowerCase()];
      } else {
        return this.iconsMap["default"];
      }
    }
  },
  methods: {
    getSize: function(_size) {
      var fSExt = new Array("Bytes", "KB", "MB", "GB"),
        i = 0;
      while (_size > 900) {
        _size /= 1024;
        i++;
      }
      return Math.round(_size * 100) / 100 + " " + fSExt[i];
    },
    getTime: function(_time) {
      return api.time_ago(new Date(_time));
    }
  }
};
</script>
