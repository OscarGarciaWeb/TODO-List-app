<template>
    <div class="modal">    
        <div class="modal">
            <div class="modal-overlay">
                <div class="modal-content">
                    <div class="modal-header">
                        <slot name="header"><h2>default header</h2> </slot>
                    </div>  
                    <div class="modal-body">
                        <slot name="body">default body</slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="btn btn-danger ml-1" @click="$emit('closeModal')">Close</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script lang="ts">
    export default { name: "BasicModal", };
</script>
  
<style>
    .modal {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        display: table;
        transition: opacity 0.3s ease;
    }

    .modal-overlay {
        display: table-cell;
        vertical-align: middle;
    }

    .modal-content {
        width: 600px;
        height: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
    }

    .modal-header {        
        margin-top: 0;
        margin: 0 auto;
        color: black;
        text-transform: uppercase;
        font-weight: bolder !important;
    }

    .modal-body {
        margin: 20px 0;
    }

</style>
  