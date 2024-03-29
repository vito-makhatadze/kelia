<template>
    <app-layout title="Content Image Create">
        <jet-form-section @submitted="createContentImage">
            <template #title>
                Content Image
            </template>
            <template #form>
                <!-- Name -->
                <div class="col-span-6 sm:col-span-4">
                    <jet-label for="tag" value="Section"/>
                    <t-input-select :options="sections" v-model="form.section"/>
                    <jet-input-error :message="form.errors.section" class="mt-2"/>
                </div>

                <div class="col-span-6 sm:col-span-4">
                    <jet-label for="tag" value="Tag"/>
                    <t-input-select :options="tags" v-model="form.tag"/>
                    <jet-input-error :message="form.errors.tag" class="mt-2"/>
                </div>
                <div class="col-span-6 sm:col-span-4">
                    <jet-label for="img_path" value="Image"/>
                    <t-input-file model-name="content-image" :default-value="contentImage?.image_src" v-model="form.image" />
                    <jet-input-error :message="form.errors.image" class="mt-2"/>
                </div>
            </template>

            <template #actions>
                <jet-action-message :on="form.recentlySuccessful" class="mr-3">
                    Saved.
                </jet-action-message>

                <jet-button :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Save
                </jet-button>
            </template>
        </jet-form-section>
    </app-layout>
</template>

<script>
import {defineComponent} from 'vue'
import JetButton from '@/Jetstream/Button.vue'
import JetFormSection from '@/Jetstream/FormSection.vue'
import JetInputError from '@/Jetstream/InputError.vue'
import JetLabel from '@/Jetstream/Label.vue'
import JetActionMessage from '@/Jetstream/ActionMessage.vue'
import AppLayout from "@/Layouts/AppLayout";
import TInputSelect from "@/Components/Form/Inputs/TInputSelect";
import TInputFile from "@/Components/Form/Inputs/TInputFile";
import {responseParse} from "@/Mixins/responseParse";
import {getData} from "@/Mixins/getData";

export default defineComponent({
    components: {
        AppLayout,
        JetActionMessage,
        JetButton,
        JetFormSection,
        JetInputError,
        JetLabel,
        TInputSelect,
        TInputFile,
    },
    props: {
        contentImage: {
          type: Object,
          default() {
              return {}
          }
        },
        tags: {
            type: Array,
            default() {
                return []
            }
        },
        sections: {
            type: Array,
            default() {
                return []
            }
        },
    },
    data() {
        return {
            form: this.$inertia.form({
                _method: 'PUT',
                tag: this.contentImage?.tag ? parseInt(this.contentImage.tag) : 10,
                section: this.contentImage?.section ? parseInt(this.contentImage.section) : 1000,
                image: this.contentImage?.image_id,
            }),
        }
    },
    methods: {
        createContentImage() {
            this.form.post(route('content-image.update',this.contentImage.id), {
                preserveScroll: true,
                forceFormData: true,
            });
        },
    },
})
</script>
