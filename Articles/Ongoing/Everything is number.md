## 1. Everything is number

In the realm of computing, the fundamental axiom is that machines do not perceive or comprehend the physical world in its raw, analog form. Instead, all interactions with reality are mediated through discrete numerical abstractions. Computers operate exclusively on binary digits, sequences of 0s and 1s, representing integers, floating-point values, or more complex algebraic structures. This reductionist paradigm implies that any real-world entity, from a tangible object to an ephemeral sensation, must be quantized and encoded into numerical formats before algorithmic manipulation can occur.

Consider, for instance, a digital photograph of the Christ the Redeemer(Cristo Redentor - RJ/Brazil). What appears as a vivid image to the human eye is, at the machine level, a two-dimensional grid of pixels, each encoded as a triplet of integers in the RGB color space: for example, a foggy orange hue might be represented as [255, 128, 64], where each value denotes the intensity of red, green, and blue channels on an 8-bit scale (0 to 255). Similarly, a GPS location pinpointing the bridge's coordinates is distilled into a pair of floating-point numbers, such as (37.7749, -122.4194), corresponding to latitude and longitude in decimal degrees. These numerical proxies enable the computer to perform operations like route optimization or image enhancement, transforming sensory data into computable forms.
## 2. The real world as numbers

### Images
Images exemplify the discretization of visual reality into numerical arrays. An image is decomposed into a raster grid of pixels, where each pixel is a vector in a color space. In the RGB model, prevalent in digital imaging, each pixel is a 3-tuple of integers:

$p=(r,g,b),r,g,b∈[0,255]\mathbf{p} = (r, g, b), \quad r, g, b \in [0, 255]p=(r,g,b),r,g,b∈[0,255]$

for 24-bit color depth. For grayscale images, this simplifies to a scalar intensity value:

$i∈[0,255]i \in [0, 255]i∈[0,255]$

representing luminance.

This numerical foundation underpins advanced processing techniques. In facial recognition, algorithms like Eigenfaces or deep convolutional neural networks (CNNs) treat the image as a high-dimensional vector in:

$Rm×n×3\mathbb{R}^{m \times n \times 3}Rm×n×3$

(for an m×nm \times nm×n image). Feature extraction involves matrix operations, such as principal component analysis (PCA), where the covariance matrix is computed:

$Σ=1N∑i=1N(xi−μ)(xi−μ)T\Sigma = \frac{1}{N} \sum_{i=1}^N (\mathbf{x}_i - \mu)(\mathbf{x}_i - \mu)^TΣ=N1​i=1∑N​(xi​−μ)(xi​−μ)T$

over pixel vectors xi\mathbf{x}_ixi​, yielding eigenvectors that capture facial variances.

Image classification, as in models like ResNet, relies on backpropagation over these numerical tensors, minimizing loss functions like cross-entropy:

L=−∑ylog⁡(y^)L = -\sum y \log(\hat{y})L=−∑ylog(y^​)

where yyy and y^\hat{y}y^​ are one-hot encoded labels and predictions derived from pixel values.

### 2.2 Sound and Voice

Acoustic phenomena, such as sound waves, are captured through analog-to-digital conversion (ADC), sampling continuous pressure variations at discrete intervals. A sound wave is represented as a time-series sequence s[t] s[t] s[t], where t t t is the sample index, and s[t] s[t] s[t] is a quantized amplitude, typically a 16-bit integer ranging from -32768 to 32767 in pulse-code modulation (PCM). The sampling rate, often 44.1 kHz for CD-quality audio, adheres to the Nyquist-Shannon theorem, ensuring frequencies up to 22.05 kHz are reconstructible.

Speech-to-text systems leverage this numerical representation via signal processing. Spectrograms transform the waveform into a time-frequency matrix using the short-time Fourier transform (STFT): S(m,ω)=∑n=−∞∞s[n]w[n−m]e−jωn S(m, \omega) = \sum_{n=-\infty}^{\infty} s[n] w[n-m] e^{-j \omega n} S(m,ω)=∑n=−∞∞​s[n]w[n−m]e−jωn, where w w w is a window function. Recurrent neural networks (RNNs) or transformers then process these matrices as input tensors, predicting phoneme probabilities. Conversely, text-to-speech (TTS) synthesizes waveforms from linguistic features, employing models like Tacotron, which generate mel-spectrograms—numerical grids of log-scaled frequencies—and invert them via vocoders like WaveNet, autoregressively sampling amplitudes conditioned on prior values.

### 2.3 Text and Language

Textual data, ostensibly symbolic, is fundamentally numerical through character encodings. ASCII maps characters to 7-bit integers (e.g., 'A' = 65), while Unicode extends this to variable-length codes like UTF-8, where a character might be encoded as a sequence of bytes, such as the emoji 😂 as [240, 159, 152, 130] in decimal. Strings are thus arrays of integers, facilitating bitwise operations.

Natural language processing (NLP) elevates this to vector spaces. Word embeddings, such as Word2Vec, represent tokens as dense vectors in Rd \mathbb{R}^d Rd (e.g., d=300), trained via skip-gram models maximizing log⁡σ(uoTvc)+∑i=1kEwi∼Pn(w)[log⁡σ(−uiTvc)] \log \sigma(\mathbf{u}_o^T \mathbf{v}_c) + \sum_{i=1}^k \mathbb{E}_{w_i \sim P_n(w)} [\log \sigma(-\mathbf{u}_i^T \mathbf{v}_c)] logσ(uoT​vc​)+∑i=1k​Ewi​∼Pn​(w)​[logσ(−uiT​vc​)], where vc \mathbf{v}_c vc​ and uo \mathbf{u}_o uo​ are center and context word vectors. Transformers in models like BERT process tokenized sequences as integer indices into embedding matrices, applying self-attention: Attention(Q,K,V)=softmax(QKTdk)V \text{Attention}(Q, K, V) = \text{softmax}\left( \frac{QK^T}{\sqrt{d_k}} \right) V Attention(Q,K,V)=softmax(dk​​QKT​)V