<template>
    <div>
        <b-nav>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script scoped>
    export default {
        props:[
            'numCorrect',
            'numTotal'
        ]
    }
</script>
