<h1>Pix2Pix GAN based Research Component</h1>

<p>The customized Pix2Pix's Architecture is trained for 56 seated Buddha statue images found on Google, and their corresponding Ground Truths that to inpaint a head for the missing head region. A detailed description of model training has stated in the submitted EC05 Document.</p>

<h2>Summary of the Trained GAN Model</h2>

<h3>Generator</h3>
<table border="1">
  <tr>
    <th>Model Component</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Input Layer</td>
    <td>Takes input images of size 256×256×3.</td>
  </tr>
  <tr>
    <td>Encoder (Downsampling)</td>
    <td>
      <ul>
        <li>Consists of 5 blocks, each containing:</li>
        <ul>
          <li>A convolutional layer with kernel size 4×4, stride 2, "same" padding, and a random normal initializer.</li>
          <li>Batch normalization (except for the first block).</li>
          <li>LeakyReLU activation.</li>
        </ul>
        <li>Number of filters doubles with each block: 64 → 128 → 256 → 512 → 512.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Bottleneck</td>
    <td>Last layer of the encoder path. Downsampled to a 512-channel representation of size 8×8.</td>
  </tr>
  <tr>
    <td>Decoder (Upsampling)</td>
    <td>
      <ul>
        <li>Consists of 4 blocks, each containing:</li>
        <ul>
          <li>Transposed convolutional layer with kernel size 4×4, stride 2, "same" padding, and a random normal initializer.</li>
          <li>Batch normalization.</li>
          <li>Dropout applied in the first upsampling layer.</li>
          <li>ReLU activation.</li>
        </ul>
        <li>Each block is concatenated with its corresponding encoder feature map (skip connections).</li>
        <li>Number of filters decreases with each block: 512 → 256 → 128 → 64.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Output Layer</td>
    <td>
      A single transposed convolutional layer with kernel size 4×4, stride 2, "same" padding, and tanh activation to generate a 256×256×3 output.
    </td>
  </tr>
  <tr>
    <td>Optimizer</td>
    <td>Adam optimizer with a learning rate of 1e-4 and β1=0.7.</td>
  </tr>
  <tr>
    <td>Loss Function</td>
    <td>Combination of Binary Cross-Entropy (GAN loss), Mean Absolute Error (L1 loss), and Structural Similarity Index (SSIM loss).</td>
  </tr>
</table>

<h3>Discriminator</h3>
<table border="1">
  <tr>
    <th>Model Component</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Input Layer</td>
    <td>Takes two input images of size 256×256×3: the real/generated image and the corresponding ground truth image.</td>
  </tr>
  <tr>
    <td>Convolutional Layers</td>
    <td>
      <ul>
        <li>Consists of 3 downsampling blocks, each containing:</li>
        <ul>
          <li>Convolutional layer with kernel size 4×4, stride 2, "same" padding, and a random normal initializer.</li>
          <li>Batch normalization.</li>
          <li>LeakyReLU activation.</li>
        </ul>
        <li>Number of filters increases with each block: 64 → 128 → 256.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Intermediate Layers</td>
    <td>
      <ul>
        <li>A padding layer after the last convolutional block.</li>
        <li>A convolutional layer with 512 filters, kernel size 4×4, stride 1, and no bias.</li>
        <li>Batch normalization and LeakyReLU activation.</li>
        <li>Another padding layer.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Output Layer</td>
    <td>A final convolutional layer with 1 filter, kernel size 4×4, stride 1, and no activation function to produce a 30×30×1 output.</td>
  </tr>
  <tr>
    <td>Optimizer</td>
    <td>Adam optimizer with a learning rate of 1e-4 and β1=0.7.</td>
  </tr>
  <tr>
    <td>Loss Function</td>
    <td>Binary Cross-Entropy.</td>
  </tr>
</table>

<h3>Other Model Details</h3>
<table border="1">
  <tr>
    <th>Category</th>
    <th>Details</th>
  </tr>
  <tr>
    <td><strong>Input Image Dimensions</strong></td>
    <td>256×256×3</td>
  </tr>
  <tr>
    <td><strong>Batch Size</strong></td>
    <td>1</td>
  </tr>
  <tr>
    <td><strong>Image Preprocessing</strong></td>
    <td>
      <ul>
        <li>Normalized to [-1, 1].</li>
        <li>Random jittering (resize, random crop to 256×256).</li>
        <li>Random horizontal flip (50% chance).</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><strong>Platform</strong></td>
    <td>Google Colab with T4 GPU enabled.</td>
  </tr>
  <tr>
    <td><strong>Checkpoints</strong></td>
    <td>Stored in <code>./training_checkpoints</code>. Saved every 5000 steps.</td>
  </tr>
  <tr>
    <td><strong>Steps</strong></td>
    <td>Training configured for 40,000 steps.</td>
  </tr>
</table>

<h3>Model Results</h3>
<img src="https://github.com/user-attachments/assets/1340d57f-7464-42cc-a5b6-784300a9837a">
<table>
    <tr>
        <th>Model Loss</th>
        <th>Score</th>
    </tr>
    <tr>
        <th>GAN Loss (BCE)</th>
        <td>1.8615</td>
    </tr>
    <tr>
        <th>L1 Loss (MAE)</th>
        <td>0.0233</td>
    </tr>
    <tr>
        <th>SSIM Loss</th>
        <td>0.1093</td>
    </tr>
    <tr>
        <th>Discriminator Loss</th>
        <td>2.6129</td>
    </tr>
</table>

<h2>Results for Test Cases</h2>
<img src="https://github.com/user-attachments/assets/00ee56cc-3a0c-40f4-959c-61bcdf317bf7">
<img src="https://github.com/user-attachments/assets/213623cc-3282-446d-85ca-69d054730b4e">
<img src="https://github.com/user-attachments/assets/e70bc308-d376-431f-85ed-05c1f99c4f47">
<img src="https://github.com/user-attachments/assets/df495bbf-c887-4aaf-8dec-dac1d50a749f">

<h2>Evaluation Metrics for Test Cases</h2>

<table>
    <tr>
        <th>Case Number</th>
        <th>Mean Absolute Error (MAE)</th>
        <th>Structural Similarity Index Measure (SSIM)</th>
    </tr>
    <tr>
        <td>01</td>
        <td>0.024844</td>
        <td>0.923690</td>
    </tr>
    <tr>
        <td>02</td>
        <td>0.017660</td>
        <td>0.943401</td>
    </tr>
    <tr>
        <td>03</td>
        <td>0.040716</td>
        <td>0.913265</td>
    </tr>
    <tr>
        <td>04</td>
        <td>0.023242</td>
        <td>0.890602</td>
    </tr>
</table>

<h2>Overall Model Evaluation</h2>

<table>
    <tr>
        <th>Average Mean Absolute Error (MAE)</th>
        <td>0.026615</td>
    </tr>
    <tr>
        <th>Average Structural Similarity Index Measure (SSIM)</th>
        <td>0.917740</td>
    </tr>
</table>
