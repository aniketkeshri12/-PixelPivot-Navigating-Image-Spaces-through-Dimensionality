# mBART Model Training and Inference

This project demonstrates training an mBART model using the Hugging Face library, fine-tuning it on a dataset, and performing inference for translation tasks. The model and tokenizer are saved and reused for inference on test sentences.

## Requirements

Ensure you have the necessary dependencies by installing them via `requirements.txt`:

```bash
pip install -r requirement.txt
```

### Dependencies:
- **torch** - PyTorch for model building and training.
- **transformers** - Hugging Face's transformers library for NLP models.
- **datasets** - For handling datasets.
- **sacrebleu** - Evaluation metric for machine translation.
- **sentencepiece** - Tokenization tool.
- **accelerate** - For accelerated training.
- **tqdm** - Progress bar for loops.
- **pandas** - For data manipulation.

## File Structure

```
project/
├── app.py                # Model training script
├── test.py               # Inference script (testing the trained model)
├── config.json           # Configuration file (parameters for training and inference)
├── logger.py             # Logger setup for consistent logging
├── logging.yml           # Logging configuration file
├── requirement.txt       # List of dependencies
├── cleaned_file.csv      # Dataset used for training
├── mbart-finetuned-nl-en # Directory for saving the fine-tuned model
│   ├── config.json       # Model configuration
│   ├── pytorch_model.bin # Trained model weights
│   └── tokenizer.json    # Tokenizer
```

## Configuration

The **`config.json`** file contains all the necessary parameters for training the model and performing inference. You can modify the values according to your needs.

Example configuration:

```json
{
    "SRC_LANG": "nl_XX",
    "TGT_LANG": "en_XX",
    "file_path": "cleaned_file.csv",
    "source_column": "title_orig_s",
    "target_column": "title",
    "output_dir": "./mbart-finetuned-nl-en",
    "evaluation_strategy": "epoch",
    "save_strategy": "epoch",
    "learning_rate": 3e-5,
    "per_device_train_batch_size": 8,
    "per_device_eval_batch_size": 8,
    "weight_decay": 0.01,
    "save_total_limit": 2,
    "num_train_epochs": 5,
    "predict_with_generate": true,
    "fp16": true,
    "report_to": "none",
    "max_input_length": 128,
    "max_target_length": 128
}
```

### Key Fields:
- **SRC_LANG**: Source language code (e.g., `nl_XX` for Dutch).
- **TGT_LANG**: Target language code (e.g., `en_XX` for English).
- **file_path**: Path to the dataset CSV file.
- **output_dir**: Directory where the fine-tuned model is saved.
- **learning_rate**: Learning rate for training.
- **num_train_epochs**: Number of training epochs.

## Usage

### Training the Model

To train the mBART model, run the `app.py` script. This will:
- Load the dataset from the CSV file.
- Tokenize the data.
- Fine-tune the model based on the configuration provided in `config.json`.
- Save the fine-tuned model and tokenizer.

Run the following command to start the training:

```bash
python app.py
```

Once training is complete, the model and tokenizer will be saved to the `output_dir` specified in `config.json`.

### Running Inference

To run inference and translate test sentences using the fine-tuned model, run the `test.py` script. It will:
- Load the trained model and tokenizer from the `output_dir`.
- Perform translation based on the test sentence.

Run the following command to perform inference:

```bash
python test.py
```

You can modify the `test_sentence` in `test.py` to translate different sentences.

## Logging

Logs are handled using Python's logging module and are configured to print to both the console and a file (`logs/application.log`). The log level can be adjusted in the **`logging.yml`** file.

### Example log entries:
```plaintext
2025-03-26 14:23:03,808 - test.py:14 - INFO - Configuration loaded from config.json
2025-03-26 14:23:03,974 - test.py:23 - INFO - Using device: cuda
2025-03-26 14:23:03,975 - test.py:34 - ERROR - Error loading model/tokenizer: Can't load tokenizer for './mbart-finetuned-nl-en'
```

---
