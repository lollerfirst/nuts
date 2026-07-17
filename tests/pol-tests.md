# Proof of Liabilities (PoL) Test Vectors

This document provides test vectors for verifying implementations of the Proof of Liabilities (PoL) specification (NUT-XX), including Merkle Mountain Range with Sums (sum-MMR) computations, right-to-left peak bagging, and epoch manifest BIP-340 Schnorr signing.

---

## 1. Node Hashing and Peak Bagging Formulations

Each node in the sum-MMR has the structure `(hash, sum)` where `hash` is 32 bytes and `sum` is an 8-byte big-endian unsigned integer (uint64).

### Parent Node Hashing

Given left child L = (hash_L, sum_L) and right child R = (hash_R, sum_R):

- sum_P = sum_L + sum_R
- hash_P = SHA256(hash_L || hash_R || bytes_8(sum_L) || bytes_8(sum_R))

### Peak Bagging Hashing (Right-to-Left)

For a list of MMR peaks P_1, P_2, ..., P_k of decreasing heights, they are recursively bagged from right to left:

- B_k = P_k
- B_i = Parent(P_i, B_i+1) for i in [1, k-1]
- The final bagged commitment root is B_1.

---

## 2. 2-Leaf MMR Tree Computation

This test vector uses two leaf nodes, representing a sum-MMR of size 2. These two leaves are merged at height 1 into a single peak root.

### Leaves

| Blinded Message B\_ (33-Byte Compressed Pubkey Hex)                  | Value | Hash (SHA256 of Raw Bytes)                                         | Leaf Index |
| :------------------------------------------------------------------- | :---- | :----------------------------------------------------------------- | :--------- |
| `02b1a03e1b10a23429fa221087e53f19001b97ad89498a44b93b3f23a851121df4` | 100   | `6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85` | 0          |
| `02c3a50646bc1a1fef3da21973b064eb6897de58231c5f3e2730bf18361592394a` | 250   | `aa80cd1d9ae985f212fd6c41cdf4c8747c92d787e9d8fd45e5d7e3f85941937f` | 1          |

### Bagged Root

- **Hash:** `90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f`
- **Sum:** `350`

### Sibling Inclusion Proofs

#### Proof for `02b1a03e...` (Index 0)

- **Leaf Index:** 0
- **Sibling Path:**
  ```json
  [
    {
      "hash": "aa80cd1d9ae985f212fd6c41cdf4c8747c92d787e9d8fd45e5d7e3f85941937f",
      "sum": 250,
      "is_left": false
    }
  ]
  ```
- **Peaks:**
  ```json
  [
    {
      "hash": "90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f",
      "sum": 350
    }
  ]
  ```

#### Proof for `02c3a506...` (Index 1)

- **Leaf Index:** 1
- **Sibling Path:**
  ```json
  [
    {
      "hash": "6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85",
      "sum": 100,
      "is_left": true
    }
  ]
  ```
- **Peaks:**
  ```json
  [
    {
      "hash": "90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f",
      "sum": 350
    }
  ]
  ```

---

## 3. 3-Leaf MMR Tree Computation

This test vector uses three leaf nodes, representing a sum-MMR of size 3.

### Leaves

| Blinded Message B\_ (33-Byte Compressed Pubkey Hex)                  | Value | Hash (SHA256 of Raw Bytes)                                         | Leaf Index |
| :------------------------------------------------------------------- | :---- | :----------------------------------------------------------------- | :--------- |
| `02b1a03e1b10a23429fa221087e53f19001b97ad89498a44b93b3f23a851121df4` | 100   | `6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85` | 0          |
| `02c3a50646bc1a1fef3da21973b064eb6897de58231c5f3e2730bf18361592394a` | 250   | `aa80cd1d9ae985f212fd6c41cdf4c8747c92d787e9d8fd45e5d7e3f85941937f` | 1          |
| `03c0029b38423f03b6d203a55e2d6778035740e40dd3d888301b3b47aede737b6f` | 500   | `95b7ec67b1f85ca98781f08fc4613559820b99f178707b29c8ebb4577aca5f40` | 2          |

### Bagged Root

- **Hash:** `2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040`
- **Sum:** `850`

### Sibling Inclusion Proofs

#### Proof for `02b1a03e...` (Index 0)

- **Leaf Index:** 0
- **Sibling Path:**
  ```json
  [
    {
      "hash": "aa80cd1d9ae985f212fd6c41cdf4c8747c92d787e9d8fd45e5d7e3f85941937f",
      "sum": 250,
      "is_left": false
    }
  ]
  ```
- **Peaks:**
  ```json
  [
    {
      "hash": "90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f",
      "sum": 350
    },
    {
      "hash": "95b7ec67b1f85ca98781f08fc4613559820b99f178707b29c8ebb4577aca5f40",
      "sum": 500
    }
  ]
  ```

#### Proof for `02c3a506...` (Index 1)

- **Leaf Index:** 1
- **Sibling Path:**
  ```json
  [
    {
      "hash": "6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85",
      "sum": 100,
      "is_left": true
    }
  ]
  ```
- **Peaks:**
  ```json
  [
    {
      "hash": "90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f",
      "sum": 350
    },
    {
      "hash": "95b7ec67b1f85ca98781f08fc4613559820b99f178707b29c8ebb4577aca5f40",
      "sum": 500
    }
  ]
  ```

#### Proof for `03c0029b...` (Index 2)

- **Leaf Index:** 2
- **Sibling Path:** `[]`
- **Peaks:**
  ```json
  [
    {
      "hash": "90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f",
      "sum": 350
    },
    {
      "hash": "95b7ec67b1f85ca98781f08fc4613559820b99f178707b29c8ebb4577aca5f40",
      "sum": 500
    }
  ]
  ```

---

## 4. Epoch Manifest Signatures

The mint periodically aggregates the roots for all keysets, creates a deterministic global digest, obtains an OpenTimestamps (OTS) receipt, and signs an epoch manifest.

### Keys and Metadata

- **Master Private Key (`seckey`):** `371b3102088ee8fa21744920b996fa717417631271730ad34269646465998245`
- **Master Public Key (`pubkey`):** `f3dd0e40dd3d888301b3b47aede737b6f9451ab451dfc05a1ae023ab4235b4dd`
- **Keyset ID:** `009a6154b71113b7`
- **Epoch Index:** `1`
- **Timestamp:** `2026-06-11T12:00:00Z`
- **Previous Global Digest:** `0000000000000000000000000000000000000000000000000000000000000000`
- **Issued MMR Size:** `3`
- **Issued MMR Root Hash:** `2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040`
- **Issued MMR Root Sum:** `850`
- **Spent MMR Size:** `0`
- **Spent MMR Root Hash:** `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` (empty MMR hash)
- **Spent MMR Root Sum:** `0`
- **Outstanding Balance:** `850`
- **Active:** `true`
- **Deactivation:** `1799999999`
- **OpenTimestamps Receipt (Hex):** `00000000000000004d4f434b5f4f54535f524543454950545f464f525f484153485f676c6f62616c5f6469676573745f6865785f76616c7565`

### Serialized Manifest String

The colon-separated UTF-8 string to sign (which excludes the `ots_receipt` and includes the `previous_global_digest`):

```
009a6154b71113b7:1:2026-06-11T12:00:00Z:0000000000000000000000000000000000000000000000000000000000000000:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:0:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855:0:850:true:1799999999
```

### Signature Computation

- **Message SHA-256 Digest:** `95dfd5da74cd81832e8eee328a263c08dfac7af0f00bf305886aa902a63c2130`
- **Auxiliary Random Data (`aux_rand`):** `b777e0270e6f6bd9302268a253ffda221ce9257a6e13349e198169745c45d72e`
- **BIP-340 Schnorr Signature (`mint_signature`):** `11b6ad3575d677db44fbe7b90125cb187f1b924cae8ccfbf7297588284a138100559342afd4b878dab56ae2e77839796fcc19ac1f0d65f81fbd3f6861911cf79`

---

## 5. Signed Transactional PoL Receipts

This section provides test vectors for the cryptographically signed Proof of Liability (PoL) receipts returned during state-transitioning actions, demonstrating the domain separation prefixes.

### Keys and Metadata

- **Keyset-amount Private Key (`seckey`):** `0000000000000000000000000000000000000000000000000000000000000001`
- **Keyset-amount Public Key (`pubkey`):** `0279be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798`
- **Target Epoch:** `12`
- **Output Blinded Message (`B'_hex`):** `02b1a03e1b10a23429fa221087e53f19001b97ad89498a44b93b3f23a851121df4`
- **Spent Input Secret Point (`Y_hex`):** `02c3a50646bc1a1fef3da21973b064eb6897de58231c5f3e2730bf18361592394a`
- **Auxiliary Random Data (`aux_rand`):** `0000000000000000000000000000000000000000000000000000000000000000`

### 5.1 Output (Issued/Active) Receipt

- **Payload String to Sign:** `Cashu_PoL_Receipt_Issued:02b1a03e1b10a23429fa221087e53f19001b97ad89498a44b93b3f23a851121df4:12`
- **Message SHA-256 Digest:** `cf056d6968059292716f12b612bcea6184a7204909da7126491ec49832edc3cb`
- **secp256k1 BIP-340 Schnorr Signature:** `31ef4e45aec5da42a7622bfbc6a8d0f9e07b562aa69092b6a2b7ea3a9b8ec92f88f4d510d488f55b00c2ea1bed0bb1f499c55eda275ffee9e0df60bf941a71b2`

### 5.2 Spent Input Receipt

- **Payload String to Sign:** `Cashu_PoL_Receipt_Spent:02c3a50646bc1a1fef3da21973b064eb6897de58231c5f3e2730bf18361592394a:12`
- **Message SHA-256 Digest:** `630865434d37eb31ee3b8d472468a7154b63a1e45d530e7bc4cf736e8b9a9a6c`
- **secp256k1 BIP-340 Schnorr Signature:** `28b635335642ac4693f4eefb068500b5360c89df907537ad4f1baa25b5de48e30fb7a00f2e6a12ea864f5fbe0c0e5a8fd2c15ada088938eba55c339e215904df`

---

## 6. Keyset Lifecycle Vectors

These vectors exercise the Keyset Lifecycle Commitments rules. Each scenario is an independent hypothetical history: 6.1 continues the keyset from section 4, and 6.2 through 6.4 each use a distinct keyset. All manifests are signed with the master keys from section 4. Auxiliary random data for every signature below is 32 zero bytes (`0000...00`). The `previous_global_digest` values are synthetic constants; the digest chain and OTS attestations are not under test here.

### 6.1 Clean Lifecycle (compliant)

Epoch 1 is exactly the manifest from section 4 (`active: true`, `deactivation: 1799999999`). Epoch 2 deactivates the keyset: `active` becomes `false`, `deactivation` is unchanged, the Issued MMR is frozen (size `3`, same root and sum), and the Spent MMR grows (size `1`, root `6711094b...`, sum `100`, outstanding `750`).

- **Epoch 2 Serialized Manifest String:**
  ```
  009a6154b71113b7:2:2026-06-12T12:00:00Z:1111111111111111111111111111111111111111111111111111111111111111:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:1:6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85:100:750:false:1799999999
  ```
- **Epoch 2 Message SHA-256 Digest:** `126a88440d303de772435aeb6bcbb3b69701edae5e29782a3a3ca0ce06bfbe14`
- **Epoch 2 BIP-340 Schnorr Signature (`mint_signature`):** `bf70bf2ddccf59925c2d954c5dcc3a55945273fcd478684732999f5c1168da30d865f882cda17e0292b4d67f28bac4523d630377e0adfa36e0c940fc6873b01a`

### 6.2 Reactivation Violation

Keyset `00b2c4d6e8fa0c1e` (`deactivation: null`): epoch 3 shows `active: false`, epoch 4 shows `active: true`. Together they satisfy the `rotation_violation` challenge with `violation_kind: reactivation`. Verifiers MUST reject epoch 4 during Step 3 lifecycle checks.

- **Epoch 3 Serialized Manifest String:**
  ```
  00b2c4d6e8fa0c1e:3:2026-06-13T12:00:00Z:2222222222222222222222222222222222222222222222222222222222222222:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:1:6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85:100:750:false:null
  ```
- **Epoch 3 Message SHA-256 Digest:** `6797dd542f4dde53121062effe58432fbb46b1cec7e2de5332d11ea44040980b`
- **Epoch 3 BIP-340 Schnorr Signature (`mint_signature`):** `7d41366b85d06f5a718fba60c1aac5b2a48a2bed4b37a821af90a84fbff60c368bea4caadac2bad385948a1d552ef28ce94b0b8ab96a780c192c8ef247e7e769`
- **Epoch 4 Serialized Manifest String:**
  ```
  00b2c4d6e8fa0c1e:4:2026-06-14T12:00:00Z:3333333333333333333333333333333333333333333333333333333333333333:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:1:6711094bb65007f6313a7c2edc4833378ef715aaf8f62ce0f9478c591dba1e85:100:750:true:null
  ```
- **Epoch 4 Message SHA-256 Digest:** `bb707de76fda62f65a35cd01746742d644f818306d168f6375b5a6344f749a44`
- **Epoch 4 BIP-340 Schnorr Signature (`mint_signature`):** `90d8d1a82962d19d4a956bc3abbb81065477f7b97959140c72260d116915922783e4db0d8e4413bc201869718bf3dec161fd50b3aee0f0face13387b053e6699`

### 6.3 Issuance-After-Lock Violation

Keyset `00c3d5e7f90b1d2f` (`deactivation: null`), both epochs `active: false`: epoch 5 has Issued MMR size `2` (root `90e8e647...`, sum `350`), epoch 6 shows the Issued MMR grown to size `3` (root `2518b42e...`, sum `850`) after the lock. Together they satisfy `rotation_violation` with `violation_kind: issuance_after_lock`.

- **Epoch 5 Serialized Manifest String:**
  ```
  00c3d5e7f90b1d2f:5:2026-06-15T12:00:00Z:4444444444444444444444444444444444444444444444444444444444444444:2:90e8e647a08f35b5b24653ab52e5d27a2deddb05d1e54d5d21777ef02036b29f:350:0:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855:0:350:false:null
  ```
- **Epoch 5 Message SHA-256 Digest:** `b29bd4a8d95ffd4fda01cbeb8beda6bebf233abf1779444305084a7bb9b6e1fb`
- **Epoch 5 BIP-340 Schnorr Signature (`mint_signature`):** `ebbb6e5d096541c3d97e9e5331a88047abcae3dc53990bc0f3fe4ce3aa37b101c005e305e07f56f2b720a86c3224a5c5e25936675c91a6443c3927de867358ea`
- **Epoch 6 Serialized Manifest String:**
  ```
  00c3d5e7f90b1d2f:6:2026-06-16T12:00:00Z:5555555555555555555555555555555555555555555555555555555555555555:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:0:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855:0:850:false:null
  ```
- **Epoch 6 Message SHA-256 Digest:** `497a7e810a7a5389e4bd14ae3ed3486f72c259f99f12c9e36087ec3139471331`
- **Epoch 6 BIP-340 Schnorr Signature (`mint_signature`):** `2355c5d251b976881d2dd29d018b74bed59d7edd3b383e4ed6c9bbfc7ec79f8e2b68cbee856f7af96d4e4df7a1a836f6ff78ad5f369a37a7c4b5a27ac2ecc697`

### 6.4 Declaration Drift Violation

Keyset `00d4e6f80a1c2e30`, both epochs `active: true` with static books: epoch 7 declares `deactivation: 1799999999`, epoch 8 declares `deactivation: 1899999999`. Together they satisfy `rotation_violation` with `violation_kind: declaration_drift`.

- **Epoch 7 Serialized Manifest String:**
  ```
  00d4e6f80a1c2e30:7:2026-06-17T12:00:00Z:6666666666666666666666666666666666666666666666666666666666666666:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:0:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855:0:850:true:1799999999
  ```
- **Epoch 7 Message SHA-256 Digest:** `162ce143e6e2488de6c9860b43286d1c8981537c6079435af75b1ede7a312545`
- **Epoch 7 BIP-340 Schnorr Signature (`mint_signature`):** `a2e3e9f0a84c9ad7e0c26a60da680cb8addb8aa098870735923c281d1cca8f47ac8e5d9e9d3736e9ad78ed379821d3182b2d754c89302da0d061a761500cd7d3`
- **Epoch 8 Serialized Manifest String:**
  ```
  00d4e6f80a1c2e30:8:2026-06-18T12:00:00Z:7777777777777777777777777777777777777777777777777777777777777777:3:2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040:850:0:e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855:0:850:true:1899999999
  ```
- **Epoch 8 Message SHA-256 Digest:** `cc60834ff8f39e029fc282abaf44dc81821ce3deeeb26e9939a31a74cfb8bf37`
- **Epoch 8 BIP-340 Schnorr Signature (`mint_signature`):** `c316f78658870c6720e7469c841b2c3d0e710870bfc7811d00dd91f87bfefea62983d6c21dab608ef1a7e3cf53e0dd3893adb47fc1717cb809e76033636d4779`

A complete `rotation_violation` challenge object for this scenario:

```json
{
  "challenge_type": "rotation_violation",
  "keyset_id": "00d4e6f80a1c2e30",
  "violation_kind": "declaration_drift",
  "manifest_a": {
    "keyset_id": "00d4e6f80a1c2e30",
    "epoch_index": 7,
    "timestamp": "2026-06-17T12:00:00Z",
    "previous_global_digest": "6666666666666666666666666666666666666666666666666666666666666666",
    "issued_mmr_size": 3,
    "issued_mmr_root_hash": "2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040",
    "issued_mmr_root_sum": 850,
    "spent_mmr_size": 0,
    "spent_mmr_root_hash": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
    "spent_mmr_root_sum": 0,
    "outstanding_balance": 850,
    "active": true,
    "deactivation": 1799999999,
    "mint_signature": "a2e3e9f0a84c9ad7e0c26a60da680cb8addb8aa098870735923c281d1cca8f47ac8e5d9e9d3736e9ad78ed379821d3182b2d754c89302da0d061a761500cd7d3"
  },
  "manifest_b": {
    "keyset_id": "00d4e6f80a1c2e30",
    "epoch_index": 8,
    "timestamp": "2026-06-18T12:00:00Z",
    "previous_global_digest": "7777777777777777777777777777777777777777777777777777777777777777",
    "issued_mmr_size": 3,
    "issued_mmr_root_hash": "2518b42edfff24ecc53c8897d1860783d1d26c41d61c378fe612cddeed877040",
    "issued_mmr_root_sum": 850,
    "spent_mmr_size": 0,
    "spent_mmr_root_hash": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
    "spent_mmr_root_sum": 0,
    "outstanding_balance": 850,
    "active": true,
    "deactivation": 1899999999,
    "mint_signature": "c316f78658870c6720e7469c841b2c3d0e710870bfc7811d00dd91f87bfefea62983d6c21dab608ef1a7e3cf53e0dd3893adb47fc1717cb809e76033636d4779"
  }
}
```
