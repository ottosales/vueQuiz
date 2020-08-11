<template>
    <div>
        <b-nav tabs class="mb-5">
            <b-nav-item disabled><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled>Total questions: 10</b-nav-item>
            <b-nav-item disabled>Counter {{numCorrect}}/{{numTotal}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default{
    props: [
        'numCorrect',
        'numTotal'
    ]
}

</script>