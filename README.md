# TensorFlow Model Static Encryption

This example is based on the 
[TensorFlow Lite BERT QA iOS Example Application](https://github.com/tensorflow/examples/).

## Purpose

This repository demonstrates the way to statically encrypt a TensorFlow model
and decrypt it at runtime. 

## How it works

The `download_resources.sh` script in `./RunScripts` is modified to encrypt
the downloaded model as the last step:

```sh
openssl enc -aes-256-cbc -nosalt -in "${MODEL_PATH}" -out "${MODEL_PATH}.aes" -pass file:"./RunScripts/encryption_key.txt" -p -iv 0
```

A sample `encryption_key.txt` is included in this example, with the 
corresponding decryption key hard-coded in 
`BertQACore/Models/ML/BertQAHandler.swift`.

### Providing your own key

You can change the encryption key with your own. To do this, follow these steps:

1. Create a text file containing your encryption password.

2. Encrypt the model by running:

```sh
openssl enc -aes-256-cbc -nosalt -in "<your-model-path.tflite>" -out "<your-encrypted-model-path.aes>" -pass file:"<your-encryption-key-path.txt>" -p -iv 0
```

3. In the output of the `openssl` utility you will see a line similar to:

   `key=5C540A8318E5C1931940C2D7334BF71BF712F86726629C5BE7886C643D29AED6`.

   Copy the key value and paste it into 
   `BertQACore/Models/ML/BertQAHandler.swift`.

4. Run and test the example.

## Reverse engineering protection

The example is not protected against reverse-engineering. To work well, it 
requires applying an anti-reverse-engineering solution.
