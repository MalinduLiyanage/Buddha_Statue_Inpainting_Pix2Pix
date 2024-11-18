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
