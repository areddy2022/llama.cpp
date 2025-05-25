# Plan: Adding TQ2 Model Support for Bonsai in llama.cpp

#### 1.1 Model Analysis
- [ ] **Download and examine Bonsai model structure**
- [ ] **Identify custom layers and scaling mechanism**
- [ ] **Study existing TQ2_0 implementation**
#### 1.2 Scale Absorption Strategy
  - Plan how to absorb `channel_scales` into weight tensors before quantization
  - Modified equation: `output = matmul(input, weight * channel_scales)`
  - Ensure mathematical equivalence while using standard TQ2_0 format
#### 2.1 Model Detection and Registration (`convert_hf_to_gguf.py`)
- [ ] **Add Bonsai model detection**
- [ ] **Implement channel-wise scale merging**
- [ ] **Handle tensor name mapping**
#### 2.3 Quantization Integration
- [ ] **Ensure TQ2_0 compatibility**
#### 3.1 Update Conversion Pipeline (`convert_hf_to_gguf_update.py`)
- [ ] **Add Bonsai to model list**
- [ ] **Update tokenizer handling for Mistral tokenizer compatibility**
#### 3.2 Validation and Testing
- [ ] **Mathematical validation**
- [ ] **Conversion testing**
- [ ] **Quality assurance**
#### 3.3 No Build System Changes Required
- [ ] **Verify compatibility**
#### 4.1 Documentation
- [ ] **Update README.md**
  - Add Bonsai to supported models list
  - Document TQ2_0 conversion for Bonsai models
- [ ] **Add conversion guide**
  ```markdown
  ## Converting Bonsai Models

  Bonsai models use channel-wise post-matmul scaling that is automatically
  absorbed into weights during conversion:

  ```bash
  python convert_hf_to_gguf.py deepgrove/Bonsai \
    --outfile bonsai.gguf \
    --outtype tq2_0
  ```

  The channel-wise scales are merged into the weight tensors, allowing
  efficient inference using the standard TQ2_0 quantization format.
  ```
- [ ] **Technical documentation**
#### 4.2 Performance Validation
- [ ] **Benchmark against original**
- [ ] **Cross-platform testing**

#### Conversion Scripts
- `convert_hf_to_gguf.py`:
  - Add `BonsaiModel` class with scale absorption logic
  - Implement `_absorb_channel_scales()` method
  - Add tensor name mapping for Bonsai's custom layers

- `convert_hf_to_gguf_update.py`:
  - Add Bonsai model to supported models list
  - Ensure Mistral tokenizer compatibility

#### Documentation
- `README.md`: Add Bonsai to supported models
- Add conversion examples and notes about scale absorption
