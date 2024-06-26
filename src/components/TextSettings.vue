<script setup>
import Button from "@/composables/Button.vue";
import CheckButton from "@/composables/CheckButton.vue";
import ColorPalette from "@/composables/ColorPalette.vue";
import Dropdown from "@/composables/Dropdown.vue";
import RadioBox from "@/composables/RadioBox.vue";
import RadioButton from "@/composables/RadioButton.vue";
import { useSettingsStore } from "@/stores/settings";
import { useViewStore } from "@/stores/view";
import PageSettings from "./PageSettings.vue";
import IconAlignCenter from "./icons/IconAlignCenter.vue";
import IconAlignLeft from "./icons/IconAlignLeft.vue";
import IconAlignRight from "./icons/IconAlignRight.vue";
import IconBackgroundColor from "./icons/IconBackgroundColor.vue";
import IconBold from "./icons/IconBold.vue";
import IconEdit from "./icons/IconEdit.vue";
import IconFontColor from "./icons/IconFontColor.vue";
import IconItalic from "./icons/IconItalic.vue";
import IconLineSpacing from "./icons/IconLineSpacing.vue";
import IconRedo from "./icons/IconRedo.vue";
import IconRotateNone from "./icons/IconRotateNone.vue";
import IconRotateVertical from "./icons/IconRotateVertical.vue";
import IconUnderline from "./icons/IconUnderline.vue";
import IconUndo from "./icons/IconUndo.vue";
import IconJustifyCenter from "./icons/IconJustifyCenter.vue";
import IconJustifyEnd from "./icons/IconJustifyEnd.vue";
import IconJustifyStart from "./icons/IconJustifyStart.vue";
import IconImage from "./icons/IconImage.vue";
import IconResizeImage from "./icons/IconResizeImage.vue";
import ImageSelect from "./ImageSelect.vue";
import ImageSettings from "./ImageSettings.vue";
import NumberInput from "@/composables/NumberInput.vue";
import TextInput from "@/composables/TextInput.vue";
import PrintRibbon from "@/features/PrintRibbon.vue";
import SaveSettings from "@/features/SaveSettings.vue";
import LoadSettings from "@/features/LoadSettings.vue";

const settings = useSettingsStore();
const view = useViewStore();

function undo() {
  settings.undo();
  view.title = settings.title;
  view.text = settings.text;
}

function redo() {
  settings.redo();
  view.title = settings.title;
  view.text = settings.text;
}
</script>

<template>
  <div class="settings-bar">
    <Button @click="undo">
      <IconUndo />
    </Button>
    <Button @click="redo">
      <IconRedo />
    </Button>
    <PrintRibbon />
    <SaveSettings />
    <LoadSettings />
    <Dropdown>
      <template v-slot:button>
        <TextInput
          :value="view.zoom + '%'"
          @change="view.setZoom"
          width="3rem"
        />
      </template>
      <RadioBox
        v-for="zoom in [50, 75, 90, 100, 125, 150, 175, 200]"
        @click="view.setZoom(zoom)"
        :checked="view.zoom == zoom"
        name="zoom"
      >
        {{ zoom }}%
      </RadioBox>
    </Dropdown>
    <div class="line"></div>
    <Dropdown>
      <template v-slot:button>
        <div class="dropdown-font">{{ settings.font }}</div>
      </template>
      <RadioBox
        v-for="font in [
          'Arial',
          'Times',
          'Courier',
          'Playfair',
          'Cormorant',
          'Amiri',
        ]"
        @click="settings.setFont(font)"
        :checked="settings.font == font"
        name="font"
      >
        {{ font }}
      </RadioBox>
    </Dropdown>
    <div class="line"></div>
    <NumberInput
      :value="settings.fontSize"
      @change="settings.setFontSize($event)"
      :options="[8, 9, 10, 11, 12, 14, 18, 24, 30, 36, 48, 60, 72, 96]"
      prop_name="font-size"
      width="2rem"
    />
    <div class="line"></div>
    <CheckButton @click="settings.toggleBold" :checked="settings.bold">
      <IconBold />
    </CheckButton>
    <CheckButton @click="settings.toggleItalic" :checked="settings.italic">
      <IconItalic />
    </CheckButton>
    <CheckButton
      @click="settings.toggleUnderline"
      :checked="settings.underline"
    >
      <IconUnderline />
    </CheckButton>
    <div class="line"></div>
    <Dropdown type="dropdown-box">
      <template v-slot:button>
        <IconAlignLeft v-if="settings.isAlignLeft" />
        <IconAlignRight v-else-if="settings.isAlignRight" />
        <IconAlignCenter v-else />
      </template>
      <RadioButton
        @click="settings.alignLeft"
        :checked="settings.isAlignLeft"
        name="align"
      >
        <IconAlignLeft />
      </RadioButton>
      <RadioButton
        @click="settings.alignCenter"
        :checked="settings.isAlignCenter"
        name="align"
      >
        <IconAlignCenter />
      </RadioButton>
      <RadioButton
        @click="settings.alignRight"
        :checked="settings.isAlignRight"
        name="align"
      >
        <IconAlignRight />
      </RadioButton>
    </Dropdown>
    <Dropdown type="dropdown-box">
      <template v-slot:button>
        <IconJustifyStart v-if="settings.isJustifyStart" />
        <IconJustifyEnd v-else-if="settings.isJustifyEnd" />
        <IconJustifyCenter v-else />
      </template>
      <RadioButton
        @click="settings.justifyStart"
        :checked="settings.isJustifyStart"
        name="justify"
      >
        <IconJustifyStart />
      </RadioButton>
      <RadioButton
        @click="settings.justifyCenter"
        :checked="settings.isJustifyCenter"
        name="justify"
      >
        <IconJustifyCenter />
      </RadioButton>
      <RadioButton
        @click="settings.justifyEnd"
        :checked="settings.isJustifyEnd"
        name="justify"
      >
        <IconJustifyEnd />
      </RadioButton>
    </Dropdown>
    <Button @click="settings.toggleRotated">
      <IconRotateNone v-if="settings.rotated" />
      <IconRotateVertical v-else />
    </Button>
    <Dropdown>
      <template v-slot:button>
        <IconLineSpacing />
      </template>
      <RadioBox
        v-for="lineHeight in [1.0, 1.15, 1.5, 2.0, 2.5]"
        @click="settings.setLineHeight(lineHeight)"
        :checked="settings.lineHeight == lineHeight"
        name="line-spacing"
      >
        {{ lineHeight }}
      </RadioBox>
    </Dropdown>
    <div class="line"></div>
    <Dropdown :indicator="false" type="dropdown-box">
      <template v-slot:button>
        <IconImage />
      </template>
      <ImageSelect />
    </Dropdown>
    <Dropdown :indicator="false" type="dropdown-box">
      <template v-slot:button>
        <IconResizeImage />
      </template>
      <ImageSettings />
    </Dropdown>
    <div class="line"></div>
    <ColorPalette :value="settings.color" @input="settings.setColor">
      <Button>
        <IconFontColor />
      </Button>
    </ColorPalette>
    <ColorPalette :value="settings.bgColor" @input="settings.setBgColor">
      <Button>
        <IconBackgroundColor />
      </Button>
    </ColorPalette>
    <Dropdown :indicator="false" type="dropdown-box">
      <template v-slot:button>
        <IconEdit />
      </template>
      <PageSettings />
    </Dropdown>
  </div>
</template>

<style scoped>
.settings-bar {
  display: flex;
  gap: 3px;
  flex-wrap: wrap;
  justify-content: center;
}

.line {
  border-left: 1px solid var(--color-border);
  margin: 0 2px;
}

.dropdown-font {
  width: 4rem;
  overflow: hidden;
  white-space: nowrap;

  text-overflow: ellipsis;
  padding: 0 3px;
}
</style>
