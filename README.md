# autoprompt

Copilot like completion for generating more effective prompts essentially for free. The real power is a nice suggestion system for prompts that are new and creative to help the user prompt the best.

This process an be applied iteratively and actively.

For now uses the OpenAI API (so you need an API key), but could easily replace with your own model (more on this later).

You create a `.env` in the same directory you run the python with the first line in the `.env` as `OPENAI_API_KEY=yourkey`.

Can leverage a vector database of historical prompts to get inspiration and help you become more creative. Prompting on steroids.

## Simple Usage

For example if your prompt was `Explain FFT` and in the past you've had good responses to
the prompts `Explain how to cook Minestrone step by step` and `Explain QED in simple terms in the voice of Richard Feynman`,
I could complete my original prompt with nice inspiration for what works!

```python
completion = autoprompt("Explain FFT", history=["Explain how to cook Minestrone step by step",
                                   				"Explain QED in simple terms in the voice of Richard Feynman"])
print(completion)
# 'in simple, step-by-step terms.'
```

Then I can see how the new autocompleted prompt fares on chatgpt

```md
user >>> Explain FFT in simple, step-by-step terms.
chatgpt >>> Sure! FFT stands for Fast Fourier Transform. It is a mathematical algorithm used to convert a time-based signal into its frequency components. Here's a step-by-step explanation of how FFT works:

1. Imagine you have a signal, which is a time-domain representation of a waveform. This signal can represent anything, such as audio, video, or even financial data.

2. The first step is to take a small portion of this signal, which is usually called a window. The window can be of a fixed length, for example, 1024 samples. This windowing process eliminates any abrupt changes at the edges of the window and helps make the signal continuous.

3. Next, we preprocess the signal by applying a mathematical function known as a window function to this small portion of the signal. Popular window functions include the Hann window or the Blackman-Harris window, which help reduce the effects of spectral leakage.

4. Once we have the windowed portion, we apply the Fast Fourier Transform algorithm to it. The algorithm breaks down the windowed portion into its component frequencies.

5. The FFT algorithm continues to halve the frequency range and recursively applies itself to each half. This process keeps dividing the frequency range until the algorithm reaches its base case, which is typically one or two samples.

6. At each division, the algorithm performs complex calculations using a mixture of real and imaginary numbers. These calculations are based on the mathematical properties of sinusoidal waves.

7. Finally, we obtain the frequency-domain representation of the windowed portion, which is a set of complex numbers representing the amplitudes and phases of the different frequency components contained within the window.

8. We can then convert these complex numbers into more useful forms like magnitudes or power spectra for each frequency component. These values can be plotted on a graph called a spectrogram, where frequency is represented on the x-axis, and amplitude or power is represented on the y-axis.

9. By repeating this process with overlapping windows across the entire signal, we can analyze the frequencies present at various points in time, giving us valuable information about the signal's frequency content and any changes that might occur over time.

And that's the basic idea behind the Fast Fourier Transform (FFT) algorithm! It allows us to analyze the frequency components of a signal quickly and efficiently.
```

## Advanced Usage

Suppose you had a whole dataset of historical prompts people have actually used. Can we use this with autoprompt instead of a hand curated history?

Yes!

But you first need a function to embed any given text into a vector, then actually embed all your history. Compose the db as a numpy array shaped `(length, vector dimensionality)` so each prompt has a vector.

Then you can easily call autoprompt like

```python
prompt_history: list[str] # actual list of prompts
vectordb: np.ndarray # embeddings of the prompt_history for each prompt
completion = vectordb_autoprompt("Explain FFT", vectordb, prompt_history)
```

Note you must pass in the embed_model that you used to embed the prompt_history into the `vectordb_autoprompt`.

Check out [`advanced.ipynb`](advanced.ipynb) for a clear example with real data.

Based on the history of prompts, you can create some funny and very creative stuff.
For example taking a subset of the [`nomic datalake`](https://github.com/nomic-ai/gpt4all-datalake) for literary related prompts

you can get new creative prompt completions like
![Screenshot 2023-07-13 at 4 46 51 PM](https://github.com/xnought/autoprompt/assets/65095341/c6f009fd-933e-472e-a99c-482a4e13d32a)

(in [`advanced.ipynb`](advanced.ipynb))
