<script>
    import Modal from "./Modal.svelte";
    import { modal } from "./stores.js";
    import Content from "./Content.svelte";
</script>

<Modal show={$modal}>
    <Content />
</Modal>
