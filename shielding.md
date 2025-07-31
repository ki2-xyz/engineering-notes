# Shielding

When shielding a taproot asset we need to make a few fundamental assumptions: 

1) since nobody has knowledge of the private key other than the asset holder, only him can create a proof of ownership with ```tapcli assets proofs po```

2) Since an asset proof produced by ```tapcli assets proofs po``` is deterministic, we compute :

- sha256(proof)
- challenge + nonce based on current block 
- signature_over(challenge || SHA256(proof_file))
- commitment C = SHA256(signature_over(challenge || SHA256(proof_file)))

## Steps

The asset holder runs:

``` tapcli assets proofs po > proof_file ```

This proves on-chain ownership via tapd. Then they compute:

```H_proof = SHA256(proof_file)```

```sig = Sign(challenge || H_proof)``` using the asset's private key

```C = SHA256(sig)```

The commitment C is included in the SNARK circuit as a public input (sig_commitment) and pptionally published on-chain when transferring the note. Commitment ```C``` serves to prove that the shielded note was legitimately created. In addition, we can publish the commitment on chain when we transfer the shielded asset. 

##  Circuit Design:

  The signature commitment is computed as:

  ```sig_commitment = SHA256(sig_preimage || zeros_256)```

  Where:
  - sig_preimage is a 32-byte private input that would contain the signature
  - zeros_256 is 32 bytes of zeros for padding

  In production, sig_preimage would be:

  ```signature_over(challenge || SHA256(proof))```

  Bind challenge to a specific Bitcoin block hash or anchor txid, this allows the circuit to verify that the prover knows a valid signature over the proof commitment, preventing proof replay attacks.
 
