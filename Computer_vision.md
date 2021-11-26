# My note for Computer vision

## Computer vision 可以用做
* image classification
* image segmentation
* object detection
* image captioning
* personal identification
* image generation
* image stylization

## Linear layer
* Linear transformation from one linear space to another.
* input object <img src="https://render.githubusercontent.com/render/math?math=x\in\mathbf{R}^{n}">
* input vector <img src="https://render.githubusercontent.com/render/math?math=\times"> transformation matrix = output vector
  * input vector: <img src="https://render.githubusercontent.com/render/math?math=1\times n">
  * transformation matrix (or weight matrix): <img src="https://render.githubusercontent.com/render/math?math=n\times k">
    * n: input number of features
    * k: output number of features
    * the weight matrix updates during training
  * output vector: <img src="https://render.githubusercontent.com/render/math?math=1\times k">
* Classification
  * output: K values
    * 如果要分類成 C 個 classes 那就要把 output dimension 設成 C
    * 然後再加上一層 logits, 範圍是 <img src="https://render.githubusercontent.com/render/math?math=-\infty\sim\infty">
      * each logits is responsible for one class
    * 最後一層 output C 個，再接上 logits 層，然後加上 softmax 層轉成 probability
      * <img src="https://render.githubusercontent.com/render/math?math=softmax(x_i) = \frac{\exp(x_i)}{\sum_j\exp(x_j)}">，其中 <img src="https://render.githubusercontent.com/render/math?math=x_i"> 是 logits，而分母的 <img src="https://render.githubusercontent.com/render/math?math=\sum_j\exp(x_j)"> 是 normalization
  * loss: cross-entropy
    * binary cross entropy: <img src="https://render.githubusercontent.com/render/math?math=loss=-w_i[y_i\log(p_i)%2B(1-y_i)\log(1-p_i)]">
      *  <img src="https://render.githubusercontent.com/render/math?math=w_i"> 是 weight
      *  <img src="https://render.githubusercontent.com/render/math?math=p_i"> 是 class = 1 的 probability
      *  <img src="https://render.githubusercontent.com/render/math?math=y_i"> 是 target value (for binary classification case 就是 0 或 1)
      *  the loss function is computed each object and the weight average
        * for binary classification, the loss only has one non-zero term 
* Regression:
  * output: one value
  * loss: L2
* A combination of linear function is also a linear functions 所以要加入 non-linearity  
