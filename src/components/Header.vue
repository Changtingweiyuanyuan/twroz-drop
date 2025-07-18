<script setup>
import {ref, computed} from 'vue'
import Dialog from '../components/feedback/Dialog.vue'
import {ElIcon} from 'element-plus'
import {Setting} from '@element-plus/icons-vue'
import {usePreferencesStore} from '../stores/preferences'
import {gtagTrackEvent} from '../utils/gtagHelper'

const preferencesStore = usePreferencesStore()

const baseUrl = import.meta.env.BASE_URL

const handleClick = event => {
	gtag('config', 'G-7BFSQ0162D', {
		page_path: baseUrl,
	})

	window.location.href = baseUrl
}

const isPreferencesModalShown = ref(false)

const preferences = ref({
	isIconButton: false,
})
</script>

<template>
	<div class="header">
		<div class="content">
			<div class="name">
				<img :src="`${baseUrl}link.gif`" />
				<h1 class="text">RO 樂園查詢工具</h1>
			</div>
			<a
				:href="`${baseUrl}`"
				class="link"
				@click.prevent="handleClick">
				掉落物查詢
			</a>
			<div class="settings">
				<div
					class="preferences"
					@click="isPreferencesModalShown = true">
					<el-icon>
						<Setting />
					</el-icon>
					<Dialog
						modal-class="preferences-block"
						:is-shown="isPreferencesModalShown"
						:is-close-btn-shown="false"
						@update:isShown="isShown => (isPreferencesModalShown = isShown)">
						<template #content>
							<div class="preferences-block__title">當前頁面偏好設定</div>
							<div class="preferences-block__content">
								<div class="option">
									<span>
										按鈕樣式:
										<span :class="{checked: !preferencesStore.isIconButton}">
											文字
										</span>
										｜
										<span :class="{checked: preferencesStore.isIconButton}">
											圖示
										</span>
									</span>
									<label class="toggle-switch">
										<input
											type="checkbox"
											v-model="preferencesStore.isIconButton" />
										<span class="slider"></span>
									</label>
								</div>
							</div>
						</template>
						<template #footer>
							<div class="preferences-block__footer">
								<div
									class="btn"
									@click.stop="
										() => {
											isPreferencesModalShown = false
											gtagTrackEvent('preferences_button_click')
										}
									">
									CLOSE
								</div>
							</div>
						</template>
					</Dialog>
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.header {
	height: 36px;
	width: -webkit-fill-available;
	position: fixed;
	top: 0;
	left: 0;
	background: #606753;
	color: #e6e6e6;
	border-bottom: 1.5px solid #b2a99f;
	z-index: 3;

	.content {
		display: flex;
		align-items: center;
		margin: auto;
		padding: 2px 40px;

		@media (min-width: 576px) {
			max-width: 540px;
		}
		@media (min-width: 768px) {
			max-width: 720px;
		}
		@media (min-width: 992px) {
			max-width: 960px;
		}
		@media (min-width: 1200px) {
			max-width: 1140px;
		}
		@media (min-width: 1400px) {
			max-width: 1320px;
		}

		.name {
			display: flex;
			justify-content: center;
			align-items: center;
			margin-right: 24px;
			user-select: none;

			img {
				width: 24px;
				height: 24px;
			}

			.text {
				background: #0f0f0f;
				border-radius: 4px;
				font-size: 12px;
				font-weight: 400;
				margin-left: 4px;
				padding: 2px 8px;
			}
		}

		a.link {
			display: none;
			color: #e6e6e6;
			text-decoration: none;
			font-size: 14px;
			line-height: 36px;
			font-weight: bold;
			cursor: pointer;

			@media (min-width: 576px) {
				display: block;
			}
		}

		.settings {
			flex-grow: 1;
			font-size: 18px;
			display: flex;
			align-items: center;
			justify-content: end;

			& > * {
				width: 20px;
				padding: 4px 6px;
				cursor: pointer;

				&:hover {
					color: #b2a99f;
				}
			}

			.preferences {
				display: flex;
				position: relative;

				&-block {
					&__title {
						font-size: 14px;
						line-height: 36px;
						outline: none;
						color: #e6e6e6;
						background: #484d3f;
						border-radius: 4px;
						width: 100%;
						text-align: center;
						margin-bottom: 12px;
						font-weight: bold;
					}
					&__content {
						width: -webkit-fill-available;
						height: 160px;
						background: #f9e7d0;
						box-shadow: inset 4px 4px 0 rgba(0, 0, 0, 0.25),
							inset -4px -4px 0 rgba(0, 0, 0, 0.25);
						padding: 12px 16px;
						display: flex;
						flex-direction: column;
						align-items: start;
						gap: 8px;
						color: #2b1c14;
						font-weight: bold;
						position: relative;

						.option {
							display: flex;
							justify-content: space-between;
							align-items: center;
							width: 100%;

							.checked {
								color: #b72c14;
							}

							.toggle-switch {
								position: relative;
								display: inline-block;
								width: 48px;
								height: 24px;
								cursor: pointer;

								input {
									opacity: 0;
									width: 0;
									height: 0;
								}

								.slider {
									position: absolute;
									cursor: pointer;
									border: 1px solid #000;
									background-color: #fff;
									box-shadow: inset -2px -2px 0 rgba(0, 0, 0, 0.25);
									border-radius: 4px;
									top: 0;
									left: 0;
									right: 0;
									bottom: 0;
									transition: 0.4s;
								}

								.slider::before {
									position: absolute;
									content: '';
									height: 18px;
									width: 18px;
									left: 3px;
									bottom: 3px;
									background-color: #606753;
									box-shadow: inset -2px -2px 0 #484d3f;
									border-radius: 4px;
									transition: 0.4s;
								}

								input:checked + .slider::before {
									transform: translateX(23px);
								}
							}
						}
					}

					&__footer {
						font-size: 16px;
						line-height: 36px;
						font-family: 'Slackey';
						outline: none;

						.btn {
							width: 100px;
							height: 40px;
							background: #f9e7d0;
							color: #392d20;
							border-radius: 4px;
							display: flex;
							justify-content: center;
							align-items: center;
							margin: auto;
							margin-top: 12px;
							cursor: pointer;

							&:hover {
								color: #000;
							}
						}
					}
				}
			}
		}
	}
}
</style>
