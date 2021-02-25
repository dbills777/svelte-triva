<script>
    fetch(
        'https://opentdb.com/api.php?amount=10'
    )
        .then((res) => {
            return res.json();
        })
        .then((res) => {
        console.log(res.results)
        })
        .catch((err) => {
            console.error(err);
        });
</script>
