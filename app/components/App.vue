<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
        <StackLayout columns="*" rows="*">
            <Image :src="img_src" />
            <Button text="Predict" @tap="queryMLKit()"/>
            <Label :text="predictions"></Label>
        </StackLayout>
    </Page>
</template>

<script>
// import * as camera from "nativescript-camera";

export default {
    data() {
      return {
        predictions: '',
        img_src: "~/assets/images/nutella_test.jpg"
      }
    },
    methods: {
        queryMLKit() {
            this.$firebase.mlkit.custommodel
            .useCustomModel({
                image: this.img_src,
                localModelFile: "~/assets/models/retrained_graph.tflite",
                labelsFile: "~/assets/models/retrained_labels.txt",
                maxResults: 5,
                modelInput: [
                    {
                    shape: [1, 224, 224, 3],
                    type: "QUANT"
                    }
                ]
            })
            .then(result => {
                console.log(result.result)
                for (var i=0; i<result.result.length; i++){
                    this.predictions.push(result.result[i]);
                }
                console.log(JSON.stringify(this.predictions))
            })
            .catch(errorMessage => {
                alert("ML Kit error: " + errorMessage);
            })
            .catch(err => {
                console.log("Error -> " + err.message);
            });
        }
    }
}
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
