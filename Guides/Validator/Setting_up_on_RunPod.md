# Guide for running a Hip Validator on Runpod

Firstly, ensure your choice of hardware meets the minimum requirements:
- 2x NVIDIA Cards with at least 20GB VRAM each. *eg. 2 * RTX 3090.*
- 8X VCPU
- 32GB RAM
- 200GB NVMe
- Python 3.8 or higher
- A Bittensor wallet with sufficient TAO for staking and registration

If you have deployed a validator pod using the provided template link ([Runpod Template](https://runpod.io/console/deploy?template=3h5nq2r5oi&ref=l9hlag5r)), everything should already be set up for you. To start the validator, follow these steps:

<em>PLEASE ENSURE YOU RESTORE YOUR COLDKEY VIA THE PUBLIC KEY. DO NOT PROVIDE YOUR MNEMONIC TO ANY SERVICE OUTSIDE YOUR CONTROL</em>

## Steps to Start the Validator

1. **Open the Runpod web interface and select the "Connect" button for your pod.**

2. **In the Runpod web interface, go to the "TCP Mappings" section and copy the port number displayed at the top left.**

3. **Navigate to the HIP Subnet directory:**
    ```bash
    cd /workspace/HIP-Subnet
    ```

4. **Restore your keys that you want to use for validation:**
    ```bash
    btcli w regen_coldkeypub --wallet.name COLD_KEY_ss58
    btcli w regen_hotkey --wallet.name COLD_KEY --wallet.hotkey HOT_KEY --mnemonic YOUR_MNEMONIC
    ```

5. **Set up the Node.js environment using nvm:**
    ```bash
    nvm use 16
    ```

6. **Start the validator script using pm2, replacing `RUNPODPORT`, `COLD_KEY`, and `HOT_KEY` with your copied port number and key args:**
    ```bash
    pm2 start --interpreter python3 --name validator -- neurons/validator.py --axon.port RUNPODPORT --wallet.name COLD_KEY --wallet.hotkey HOT_KEY --netuid 36 --logging.debug --logging.trace
    ```

7. **Save the pm2 process list to ensure the validator starts automatically on pod restarts:**
    ```bash
    pm2 save
    ```

Your validator should now be up and running on Runpod!

## Notes
- Make sure to replace `RUNPODPORT`, `COLD_KEY`, and `HOT_KEY` with your actual port number and key args in the `pm2 start` command.
- With pm2, your validator will automatically restart if there are any interruptions to the pod.

## Managing the Validator Process with pm2

- **List all running processes:**
    ```bash
    pm2 list
    ```

- **View the logs of the validator process:**
    ```bash
    pm2 logs validator
    ```

- **Stop the validator process:**
    ```bash
    pm2 stop validator
    ```

- **Start the validator process:**
    ```bash
    pm2 start validator
    ```

- **Restart the validator process:**
    ```bash
    pm2 restart validator
    ```

Remember to run `pm2 save` after making any changes to the process list to ensure they persist across pod restarts.

That's it! You should now have a validator up and running on your Runpod template.
