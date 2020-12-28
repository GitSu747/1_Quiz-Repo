<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>Quiz App second</b-nav-item>
            <b-nav-item disabled>Correct : {{numCorrect}}/{{numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>
<script>
    export default {
        props: [
            'numTotal', 'numCorrect'
        ]
    }

</script>