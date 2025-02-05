<!--
  - @copyright Copyright (c) 2023 Louis Chemineau <louis@chmn.me>
  -
  - @author Louis Chemineau <louis@chmn.me>
  -
  - @license AGPL-3.0-or-later
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<template>
	<LMap class="location-map"
		:zoom="previewZoom"
		:center="center"
		:options="{
			scrollWheelZoom: false,
			zoomControl: false,
			dragging: false,
			attributionControl: false,
		}"
		@scroll.prevent="">
		<LTileLayer :url="url" />
		<LControlAttribution position="bottomright"
			:prefix="attribution" />
		<LMarker :lat-lng="center">
			<LTooltip :options="{
				direction: 'top',
				permanent: 'true',
				offset: [-16,-14]}">
				{{ name }}
			</LTooltip>
		</LMarker>
	</LMap>
</template>

<script>
import {
	LControlAttribution,
	LTooltip,
	LMap,
	LMarker,
	LTileLayer,
} from 'vue2-leaflet'
// Leaflet icon patch
import 'leaflet-defaulticon-compatibility/dist/leaflet-defaulticon-compatibility.webpack.css' // Re-uses images from ~leaflet package
import 'leaflet/dist/leaflet.css'

// eslint-disable-next-line
import 'leaflet-defaulticon-compatibility'

export default {
	name: 'LocationMap',
	components: {
		LControlAttribution,
		LTileLayer,
		LMap,
		LMarker,
		LTooltip,
	},
	props: {
		/**
		 * The latitude of the location
		 */
		latitude: {
			type: Number,
			required: true,
		},

		/**
		 * The longitude of the location
		 */
		longitude: {
			type: Number,
			required: true,
		},

		/**
		 * The name of the location
		 */
		name: {
			type: String,
			default: '',
		},
	},
	data() {
		return {
			url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
			// The zoom level of the map in the messages list
			previewZoom: 13,
			attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
		}
	},
	computed: {
		/** @return {[number, number]} */
		center() {
			return [this.latitude, this.longitude]
		},
	},
}
</script>

<style scoped lang="scss">
.location-map {
	position: relative;
	margin: 16px;
	border-radius: var(--border-radius-large);
	height: 250px;
	width: 90%;
}
</style>
