# Projeto de Processamento de Imagem em Python (Sem Bibliotecas Externas para Pixels)

Este projeto demonstra conceitos de processamento de imagem em Python, convertendo uma imagem colorida em escala de cinza e, em seguida, em uma imagem binária (preto e branco). O código foi desenvolvido com foco em orientação a objetos e sem o uso de bibliotecas externas para manipulação de pixels (como Pillow ou OpenCV), exceto para a leitura das imagens e exibição das imagens com matplotlib.

## Estrutura do Projeto

O projeto é organizado em classes que encapsulam diferentes aspectos do processamento de imagens:

*   `Image`: Classe pai que define a estrutura base para as classes de imagem.
*   `ImageGrayScale`: Classe que herda de `Image` e implementa a conversão para escala de cinza.
*   `ImageBinary`: Classe que herda de `Image` e implementa a binarização da imagem.
*   `FileManagement`: Classe que lida com a leitura de arquivos de imagem do Google Drive e com a obtenção das dimensões da imagem.

### Classe `Image`

**Propósito:** Define a estrutura base para classes de imagem.

**Atributos:**

*   `width`: (int) Largura da imagem.
*   `height`: (int) Altura da imagem.
*   `imageBytes`: (bytes) Dados brutos da imagem.

**Métodos:**

*   `__init__(self, width, height, imageBytes)`: Construtor da classe. Inicializa os atributos da imagem.
*   `isImage(self, imageBytes)`: Verifica se os dados da imagem são válidos.
*   `showImage(self, arr, title, cmap='gray')`: Exibe a imagem usando Matplotlib.

### Classe `ImageGrayScale`

**Propósito:** Herda de `Image` e implementa a conversão de uma imagem colorida para escala de cinza.

**Atributos:**

*   Herda os atributos `width`, `height` e `imageBytes` da classe `Image`.
*   `imageGray`: (array NumPy) Armazena a imagem resultante em tons de cinza.
*   `weightRed`: (float) Peso para o canal vermelho na conversão para escala de cinza (valor: 0.2989).
*   `weightGreen`: (float) Peso para o canal verde na conversão para escala de cinza (valor: 0.5870).
*   `weightBlue`: (float) Peso para o canal azul na conversão para escala de cinza (valor: 0.1140).

**Métodos:**

*   `__init__(self, width, height, imageBytes)`: Construtor da classe. Inicializa os atributos e executa a conversão para tons de cinza.
*   `_rgbToGrayscale(self)`:
    * Converte uma imagem RGB para escala de cinza usando a média ponderada, com a fórmula: `Cinza = 0.2989 * Vermelho + 0.5870 * Verde + 0.1140 * Azul`, sem utilizar bibliotecas externas para manipulação de pixels.
     * Este método utiliza a biblioteca `Pillow` (PIL) para abrir a imagem a partir dos bytes, converte para array numpy, e realiza a transformação.
*   `showImage(self, arr, title, cmap='gray')`: Exibe a imagem em escala de cinza.

### Classe `ImageBinary`

**Propósito:** Herda de `Image` e implementa a binarização de uma imagem em escala de cinza.

**Atributos:**

*   Herda os atributos `width`, `height` e `imageBytes` da classe `Image`.
*   `imageBinary`: (array NumPy) Armazena a imagem binarizada (preto e branco).
*   `threshold`: (int) Limiar usado para binarização (valor padrão: 128).

**Métodos:**

*   `__init__(self, width, height, image_byte, gray_image)`: Construtor da classe. Inicializa os atributos e realiza a binarização.
*   `_binarizeImage(self, gray_image, threshold)`: Converte a imagem em escala de cinza para uma imagem binária usando um limiar.
*   `showImage(self, arr, title, cmap='binary')`: Exibe a imagem binária.

### Classe `FileManagement`

**Propósito:** Lida com o carregamento de arquivos de imagem do Google Drive e com a obtenção das dimensões da imagem.

**Atributos:**

*   `imagePath`: (string) Caminho da imagem no Google Drive.
*   `imageBytes`: (bytes) Dados brutos da imagem.
*   `width`: (int) Largura da imagem (obtida do cabeçalho da imagem).
*   `height`: (int) Altura da imagem (obtida do cabeçalho da imagem).
* `_drive_mounted`: (bool) Controla se o drive foi montado, para evitar montagens duplicadas do google drive.

**Métodos:**

*   `__init__(self, image_path)`: Construtor da classe. Monta o Google Drive e lê a imagem a partir do caminho especificado.
*   `readImageFromDrive(self)`: Lê a imagem do Google Drive e retorna os bytes.
    * Imprime os bytes iniciais do arquivo e o tamanho total da imagem, para debug
*   `getImageDimensions(self)`: Procura pelos marcadores SOF0 ou SOF2 no cabeçalho da imagem JPEG para obter as dimensões (largura e altura) da imagem, e tem um tratamentos de erro mais detalhado para imagens JPEG.
*   `showImage(self)`: Exibe a imagem original carregada do Google Drive.

### Orientação a Objetos (OO)

O código demonstra os seguintes princípios de OO:

*   **Encapsulamento:** As classes organizam dados (atributos) e métodos em unidades coesas.
*   **Herança:** `ImageGrayScale` e `ImageBinary` herdam de `Image`, reutilizando seu comportamento base e estendendo a funcionalidade.
*   **Polimorfismo:** O método `showImage` pode ser usado tanto para objetos de `ImageGrayScale` quanto de `ImageBinary`, pois eles são subclasses de `Image`.
*  **Classe estática**: a variável `_drive_mounted` da classe `FileManagement` é uma variável estática que é compartilhada por todos os objetos da classe, garantindo que o drive só seja montado uma vez.

### Como Usar o Depurador `pdb.set_trace()`

O código utiliza `pdb.set_trace()` em alguns pontos estratégicos para facilitar a depuração. `pdb` é o depurador do Python.

1.  **Inserção do Breakpoint:** Para usar o depurador, coloque a linha `pdb.set_trace()` na posição do código que você quer analisar.
2.  **Execução do Código:** Quando o código atingir essa linha, o depurador será ativado e irá te mostrar o prompt `(pdb)`.
3.  **Comandos do `pdb`:** Você pode usar comandos como:
    *   `n` (next): Executa a próxima linha de código.
    *   `s` (step): Entra em uma função chamada.
    *   `c` (continue): Continua a execução até o próximo breakpoint ou o final do programa.
    *   `p <variável>` (print): Imprime o valor da variável.
    *   `q` (quit): Abandona a depuração e termina a execução.
    *   `h` (help): Exibe a ajuda com os comandos.

### Exemplo de Uso:

```python
# Exemplo de uso:
imagePath = '/content/drive/MyDrive/lena/lena.jpg'

fileManager = FileManagement(imagePath)
fileManager.showImage()

width = fileManager.width
height = fileManager.height

if fileManager.imageBytes:
    print(f"Largura da imagem: {width}")
    print(f"Altura da imagem: {height}")

    imageGray = ImageGrayScale(width, height, fileManager.imageBytes)
    imageGray.showImage(imageGray.imageGray, "Imagem em Tons de Cinza", cmap='gray')

    imageBinary = ImageBinary(width, height, fileManager.imageBytes, imageGray.imageGray)
    imageBinary.showImage(imageBinary.imageBinary, "Imagem Binarizada", cmap='binary')
