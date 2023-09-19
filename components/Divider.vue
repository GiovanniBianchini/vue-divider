<script setup lang="ts">
/**
 * This is a basic divider components where you can set
 * an empty space or a separator line for desktop, tablet and smartphone resolution
 */

/**
 * Define props value that will be passed from components use
 */
const props = defineProps({
    desktop: String, /* string as number because it contains also the unit */
    tablet: String, /* string as number because it contains also the unit */
    smartphone: String, /* string as number because it contains also the unit */
    dividerColor: String, /* color value */
    separatorLine: Boolean, /* show separator line or not */
    separatorPosition: String, /* left, center, right */
    separatorWidth: String, /* string as number because it contains also the unit */
    separatorHeight: String, /* string as number because it contains also the unit */
    separatorColor: String /* color value */
});

/**
 * Define a var to set up divider height based on viewport width. 
 * It'll start with the desktop value to avoid a unwanted behavior because of TTFB
 */
let size:any = ref(props.desktop);

/**
 * This function will set up divider size based on viewport width
 */
const checkScreenSize = ():void => {
    let windowWidth = window.innerWidth;
    if (windowWidth >= 992) {
        props.desktop ? size.value = props.desktop
        : '';
    }
    if (windowWidth < 992){
        props.tablet ? size.value = props.tablet:
        props.desktop ? size.value = props.desktop
        : '';
    }
    if(windowWidth <= 576){
        props.smartphone ? size.value = props.smartphone:
        props.tablet ? size.value = props.tablet:
        props.desktop ? size.value = props.desktop
        : '';
    }
}

/**
 * After app has mounted, launch checkScreenSize() to check screen resolution at startup.
 * Every time window resize, launch checkScreenSize() to check screen resolution
 */
onMounted(() => {
    checkScreenSize();
    window.addEventListener('resize', ():void => {
        checkScreenSize();
    });
});
</script>

<template>
    <div class="ui-divider" :style="[
        {'height': size},
        dividerColor ? {'background-color': dividerColor} : '',
        separatorPosition == 'left' ? {'justify-content': 'start'} : '',
        separatorPosition == 'center' ? {'justify-content': 'center'} : '',
        separatorPosition == 'right' ? {'justify-content': 'end'} : ''
    ] 
    ">
        <div class="separator" :style="[
            separatorLine ? {'display': 'block'} : '',
            separatorWidth ? {'width': separatorWidth} : '',
            separatorHeight ? {'height': separatorHeight} : '',
            separatorColor ? {'background-color': separatorColor} : ''
        ]"></div>
    </div>
</template>

<style scoped>
.ui-divider {
    align-items: center;
    display: flex;
}
.ui-divider .separator{
    display: none;
}
</style>
