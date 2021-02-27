    <script>
  import { CustomInput, Form, FormGroup, Label, Button } from 'sveltestrap';
  export let startGame;
  
</script>
<Form>
    <FormGroup>
        <Label for="exampleCustomSelect">Select a Questions Category</Label>
        <CustomInput type="select" id="exampleCustomSelect" name="customSelect">
            <option value="">Select</option>
            <option value="9">General Knowledge</option>
            <option value="14">Entertainment: Television</option>
            <option value="21">Sports</option>
            <option value="22">Geography</option>
            <option value="23">History</option>
        </CustomInput>
    </FormGroup>
    <FormGroup>
        <Label for="exampleCustomSelect">Select a Difficulty Level</Label>
        <CustomInput type="select" id="exampleCustomSelect" name="customSelect">
            <option value="">Select</option>
            <option value="easy">Easy</option>
            <option value="easy">Medium</option>
            <option value="easy">Hard</option>
        </CustomInput>
    </FormGroup>
    <Button class='warning' on:click={startGame()}  > Start Game </Button>
</Form>