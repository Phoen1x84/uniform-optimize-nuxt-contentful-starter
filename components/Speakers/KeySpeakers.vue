<template>
    <section>
        <h2>Speakers</h2>
        <ul>
            <li>
                <article>
                    <img src="" alt="">
                    <h3>firstname surname</h3>
                    <p>example text here....</p>                    
                </article>
            </li>
        </ul>
    </section>    
</template>
<script>
import { Personalize } from '@uniformdev/optimize-tracker-vue';
import { contentfulOptimizeListReader } from '@uniformdev/optimize-tracker-contentful';

export default {
    name: 'key-speakers',
    components: {
        Personalize
    },
    computed: {
        variations() {
            debugger;
            contentfulOptimizeListReader(this.fields.unfrmOptP13nList);
        }
    }
}
</script>