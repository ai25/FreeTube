<template>
  <ft-card class="watchVideoInfo">
    <div>
      <p
        class="videoTitle"
      >
        {{ title }}
      </p>
      <div
        class="channelInformation"
      >
        <div
          class="profileRow"
        >
          <div>
            <router-link
              class="channelName"
              :to="`/channel/${channelId}`"
            >
              <img
                :src="channelThumbnail"
                class="channelThumbnail"
                @click="goToChannel"
              >
            </router-link>
          </div>
          <div>
            <router-link
              style="text-decoration:none; color:#FFF;"
              :to="`/channel/${channelId}`"
            >
              <div
                class="channelName"
                @click="goToChannel"
              >
                {{ channelName }}
              </div>
            </router-link>
            <ft-button
              v-if="!hideUnsubscribeButton"
              :label="subscribedText"
              class="subscribeButton"
              background-color="var(--primary-color)"
              text-color="var(--text-with-main-color)"
              @click="handleSubscription"
            />
          </div>
        </div>
      </div>
    </div>
    <div>
      <div class="datePublished">
        {{ publishedString }} {{ dateString }}
      </div>
      <div class="viewCount">
        {{ parsedViewCount }}
      </div>
      <div
        v-if="!hideVideoLikesAndDislikes"
        class="likeBarContainer"
      >
        <div
          class="likeSection"
        >
          <div>
            <span class="likeCount"><font-awesome-icon icon="thumbs-up" /> {{ parsedLikeCount }}</span>
          </div>
        </div>
      </div>
      <!--
      // Uncomment if suitable solution for bringing back dislikes is introduced
      <div
        v-if="!hideVideoLikesAndDislikes"
        class="likeBarContainer"
      >
        <div
          class="likeSection"
        >
          <div
            class="likeBar"
            :style="{ background: `linear-gradient(to right, var(--accent-color) ${likePercentageRatio}%, #9E9E9E ${likePercentageRatio}%` }"
          />
          <div>
            <span class="likeCount"><font-awesome-icon icon="thumbs-up" /> {{ parsedLikeCount }}</span>
            <span class="dislikeCount"><font-awesome-icon icon="thumbs-down" /> {{ parsedDislikeCount }}</span>
          </div>
        </div>
      </div>
      -->
      <div class="videoOptions">
        <ft-icon-button
          v-if="!isUpcoming"
          :title="$t('Video.Save Video')"
          icon="star"
          class="option"
          :theme="favoriteIconTheme"
          @click="toggleSave"
        />
        <ft-icon-button
          v-if="externalPlayer !== ''"
          :title="$t('Video.External Player.OpenInTemplate').replace('$', externalPlayer)"
          icon="external-link-alt"
          class="option"
          theme="secondary"
          @click="handleExternalPlayer"
        />
        <ft-icon-button
          v-if="!isUpcoming && downloadLinks.length > 0"
          ref="downloadButton"
          :title="$t('Video.Download Video')"
          class="option"
          theme="secondary"
          icon="download"
          :return-index="true"
          :dropdown-options="downloadLinkOptions"
          @click="handleDownload"
        />
        <ft-icon-button
          v-if="!isUpcoming"
          :title="$t('Change Format.Change Video Formats')"
          class="option"
          theme="secondary"
          icon="file-video"
          :dropdown-options="formatTypeOptions"
          @click="handleFormatChange"
        />
        <ft-share-button
          v-if="!hideSharingActions"
          :id="id"
          :get-timestamp="getTimestamp"
          :playlist-id="playlistId"
          class="option"
        />
      </div>
    </div>
  </ft-card>
</template>

<script src="./watch-video-info.js" />
<style scoped src="./watch-video-info.sass" lang="sass" />
