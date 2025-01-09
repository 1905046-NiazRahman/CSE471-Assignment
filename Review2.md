# Are Language Models Actually Useful for Time Series Forecasting?


>*This review of [this blog](https://github.com/farhanitrate35/Blogs/blob/main/Are%20Language%20Models%20Actually%20Useful%20for%20Time%20Series%20Forecasting%3F/Review_Blog.md) is written by [1905009 - Sidratul Muntaha Khan](https://github.com/Nahin009) | [1905010 - Md Muhaiminul Islam Nafi](https://github.com/nafiislam) | [1905046 - Niaz Rahman](https://github.com/1905046-NiazRahman) from CSE, BUET*


The blog post provides a good overview of the research paper [Are Language Models Actually Useful for Time Series Forecasting?](https://openreview.net/forum?id=DV15UbHCY1) from NeurIPS 2024, discussing the potential and limitations of large language models (LLMs) like GPT-4 in handling time series forecasting. And the conclusion is that LLMs bring little to no benefit for the task, and are significantly more costly. 

## Strengths
1. The motivation of the paper is stated in a very clear and interesting way in the blog. 
2. The blog also states important sections and subsections of the paper nicely, which gives a clear overview of the whole paper.
3. The writings are quite standard and fun to read.

## Weaknesses
### Major Issues
1. The 3rd figure showing a table about a portion of experimental results is somewhat unclear. It would be great if more description was stated, like the “# Wins” refers to the number of times the method performed best among 26 cases ( 13 datasets each evaluated with MAE and MSE). And “# Params” is the number of model parameters. Also in the figure, red coloring at "# Wins" signifies the best performance, while red at "# Params" suggests the highest computational cost, which is less desirable. Including these clarifications would make the figure easier to understand. Overall, adding captions with proper descriptions to each figure would enhance its understanding better.
  
2. In the Evaluation Metrics section of the blog, it is stated that Mean Absolute Error (MAE) and Root Mean Square Error (RMSE) were used in the paper, but actually, RMSE is not used; rather Mean Squared Error (MSE) is used in the paper.
  
3. In the 3rd point of methodology, it is mentioned that the researchers evaluated “Forecasting”, “Classification” and  “Reasoning” in the paper. But actually, the forecasting is only evaluated, and the other two are not evaluated, which is stated as a limitation in Appendix A of the paper.
  
4. There are three methods used in the paper that use LLMs for time series forecasting: OneFitsAll, Time-LLM, and CALF. The names are present in the 3rd figure, also these are some major experimental topics, but any mention of these is not present in the blog.
  
5. The blog clearly depicts that LLMs are not useful to forecast time series. But it doesn't state the reasoning about where the performance is coming from if LLM is not present. This is stated in the 4.6 section of the main paper and is a very essential topic to understand the underlying theory of time series forecasting.

6. The blog post mentions that the researchers conducted ablation studies, but it does not provide specifics about the types of ablations performed. The paper describes three specific ablation methods: removing the LLM component, replacing it with an attention layer, and replacing it with a transformer block. The figures in the key contribution section state these terms, but there are no mentions about these ablation methods in the blog. Including these details in the blog post would significantly enhance its informativeness.

### Minor Issues
1. The first diagram in the blog is also taken from the mentioned [presentation](https://www.youtube.com/watch?v=_7v5ICY0L_c) in the blog. And the diagram is not present in the paper, which can be misleading and so needs to be mentioned.
  
2. There is a typo in the sentence: 'The architectural variations and their results as found and depicted by the authors of the paper clearly indicates...' where 'indicates' should be 'indicate'.
  
3. If the methodology section was stated before the key findings section, it would be quite better for the readers to understand the flow and contexts clearly.

4. There is a confusion in title. The actual title is "Are Language Models Actually Useful for Time Series Forecasting?". But the title is written as "Are Large Language Models Useful for Time Series Forecasting?" in the blog. There is a difference between "Language Models" And "Large Language Models".  This change could lead to confusion regarding the scope of the findings. Though the paper is focused on LLM component, the paper's findings suggest that LLMs may not provide significant advantages over simpler models in time series forecasting. By changing the title, there is a risk of misrepresenting the scope and implications of the research. It may be beneficial to retain the original title to accurately reflect the content and findings of the research.

By incorporating these improvements, the blog post could become a more comprehensive and insightful resource for readers interested in understanding the paper's findings and the potential role of LLMs in time series analysis.
