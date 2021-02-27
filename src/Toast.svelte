<script >
  import { Toast, ToastBody, ToastHeader } from 'sveltestrap';
  export let remaining;
  export let answered;
  export let score;
  $: score
</script>


  <div class="p-3 bg-secondary mb-3">
    <Toast class="mr-1" secondary>
      <ToastHeader>Questions Answered: {answered}</ToastHeader>
      <ToastHeader>Questions Remaining: {remaining}</ToastHeader>
    </Toast>
  </div>
