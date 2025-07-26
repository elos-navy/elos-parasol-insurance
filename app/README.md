# Application image

## Building

You need to download two models using huggingface-cli:

huggingface-cli download nomic-ai/nomic-embed-text-v1 --local-dir ./nomic-embed-text-v1 --local-dir-use-symlinks False
huggingface-cli download nomic-ai/nomic-bert-2048 --local-dir ./nomic-bert-2048 --local-dir-use-symlinks False

Then you need to copy some files from one to another:

cp /opt/app-root/src/nomic-bert-2048/*.py /opt/app-root/src/nomic-embed-text-v1/

Than you have to edit nomic-embed-text-v1/config.json to point to local paths like this:

"auto_map": {
  "AutoConfig": "configuration_hf_nomic_bert.NomicBertConfig",
  "AutoModel": "modeling_hf_nomic_bert.NomicBertModel",
  "AutoModelForMaskedLM": "modeling_hf_nomic_bert.NomicBertForPreTraining"
}

The npm build happens during the image build. To do it successfully, you may have to augment the limits on open files in your system. Ex:

`podman build --no-cache --ulimit nofile=10000:10000 -t elos-lab-insurance-claim-app:1.1 .`
