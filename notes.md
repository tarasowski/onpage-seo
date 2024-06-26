# SurferSEO / WriteSonic

> It gives you a game plan of going in when you go to create your content, so when you optimize it much higher, you have higher chances of ranking, cause you are doing it exactly what Google likes based on upon some of your closest competitors in the top 10

## SurferSEO overview
Source: https://www.youtube.com/watch?v=m48x-ulA4pQ
- for which keyword you want to rank for
- import from url (usually once the url starts to rank after a month you can come back and optimize it)
- write your own content
- auto-optmize to get the score up
- suggestions (basic seo onpage analyzer)
- - word count (avg. count)
  - headings (avg. count)
  - paragraphs (avg. count)
  - images (avg. count)
  - terms suggestions (more suggestions, what else you can include)

## How to research keywords with surferseo
Source: https://www.youtube.com/watch?v=s4LpbKioNEU
- input keyword
- choose the country
- building a cluster of keywords
- - shows the competitors (pick your competitors, you want to outrank with a toggle)
  - based on the competitors it suggests the keyword structure (avg. words, number of heading, number of paragraphs, number of images)
  - it then lists the keywords the competitors use
  - topics & questions: are ideas that one can use for a headline, paragraph etc. (what people ask from google, competitors content and suggestion from the surfer)
- once cluster chosen you can use these keywords to analyze the content
- metrics
- - monthly search volume
  - total traffic
  - keyword difficulty
- the onpage analyzer
- - based on the suggestions from the step of showing the competitors it calculates the score (how often which keywords, headlines, images should be included in the text)
  - lists all keywords and shows the keywords that are in the text green and other keywords red that you need to include it
  - it shows the keyword density and how often they should be included into the text

## SurferAI one-click article review
Source: https://www.youtube.com/watch?v=wuzHHG1dxLU
- first creates the outline (creates the headings)
- then you write the content
- it shows then the content score after the content is written

## SurferSEO Jaster mode ✅ (best explanation)
Source: https://www.youtube.com/watch?v=Q-BZ5TfJZuA
- first you **input the keyword** or import content from the url
- pick the country you want to rank in
- getting suggestions and keyword **ranking difficulty** and the volume
- it then goes
- - scrapes the **top 10 rankings**
  - does the keyword analysis (average keyword density)
  - does the content structure analysis (average headings, paragraphs, images)
  - suggest the keywords that should be in the content (the occurence of each keywords)
  - does recalculates the score on the fly based on the information retrieved from the first step
- feels like the gamification when you change the content you level up the score
- when new keyword is added it gets marked as green, so you can visually see what's missing
- **customize settings feature**
- - displays the competitors that were taken into consideration for ranking
  - you can swith on/off the competitors you want to include into the analysis to create the content score + suggestions
  - based on the competitors it **recalculates the content structure**
  - - amount of words
    - numbers of headings
    - number of paragraphs
    - number of images
  - - terms to use
    - it includes all important terms that are used on other competitors websites (based on occurence)
  - - topics and questions
    - auto suggestion list and what other people aks from google search
  - search term input field, you can use to search for suggested keywords on the fly

# WriteSonic SEO Checker & Optimizer
Source: https://www.youtube.com/watch?v=H5mYvG76qZ0
- when you provide content it suggest relevant keywords that you can use as your primary keywords to optimize the content
- pick the country you want to target (it goes to google and searches for the competitors)
- when you input the keyword (you can input multiple keywords, but the first one will be treated as your primary one)
- then it suggest the seo score:
- - number of words (current vs. the range it should be)
  - number of headlines (current vs. the range it should be)
  - number of paragraphs (current vs. the range it should be)
  - number of images (current vs. the range it should be)
  - competitor keywords:
  - - they show the volume for the keyword
    - they show the keywords the competitors **are ranking for** (shows the suggestions how many time the keyword should be present at your article)
    - they show longtail keywords, they keywords you should optimize for
    - they show **common competitor terms** (the intersection of keywords) <- **these are the keywords that needs to be included in the text**
- how do they come up with the score?
- - they go to google and search the competitors that rank for the specific keyword
  - first five or first 10 results
  - based on that they calcualte the averages
- **automatically optimize the article by clicking on a simple button**


# POC
1. Hook up to the google api to search the keywords the user inputs
2. Get the top 10 results from the api
3. Extract the content of each website
4. Count avg. words, avg number of headings, number of paragraphs, number of images
5. Use this script: https://github.com/tarasowski/trigrams-corpus-analysis to generate n-grams (1,2,3)
6. Then use the 3n-grams and get the volume of these keywords and display how often the keywords appear on competitors website
7. Use 3n-grams + the average acount of words, number of headings and number of paragraps and number of images to calculate the score how well the content is optimized from 0 to 100
8. Use the following formula to calculate / recalculate the score on the fly

```
// Function to calculate the optimization score
function calculateOptimizationScore(W, H, P, I, Fk, Nk, weights) {
    // Define maximum observed values
    const Wmax = 1000; // Example value, replace with actual maximum observed value
    const Hmax = 20;   // Example value, replace with actual maximum observed value
    const Pmax = 50;   // Example value, replace with actual maximum observed value
    const Imax = 10;   // Example value, replace with actual maximum observed value
    const Fkmax = 100; // Example value, replace with actual maximum observed value
    const Nkmax = 1000; // Example value, replace with actual maximum observed value
    
    // Destructure weights
    const { wWeight, hWeight, pWeight, iWeight, fkWeight, nkWeight } = weights;

    // Calculate individual terms
    const term1 = (W / Wmax) * wWeight;
    const term2 = (H / Hmax) * hWeight;
    const term3 = (P / Pmax) * pWeight;
    const term4 = (I / Imax) * iWeight;
    const term5 = (Fk / Fkmax) * fkWeight;
    const term6 = (Nk / Nkmax) * nkWeight;

    // Calculate total score
    const score = term1 + term2 + term3 + term4 + term5 + term6;
    
    return score;
}

// Example usage
const W = 800; // Example value for average word count
const H = 10;  // Example value for average number of headings
const P = 30;  // Example value for average number of paragraphs
const I = 5;   // Example value for average number of images
const Fk = 50; // Example value for frequency of keyword across competitors
const Nk = 500; // Example value for total number of n-grams containing keyword
const weights = {
    wWeight: 0.2, // Example weight for word count
    hWeight: 0.1, // Example weight for number of headings
    pWeight: 0.2, // Example weight for number of paragraphs
    iWeight: 0.1, // Example weight for number of images
    fkWeight: 0.2, // Example weight for frequency of keyword
    nkWeight: 0.2  // Example weight for total number of n-grams
};

const optimizationScore = calculateOptimizationScore(W, H, P, I, Fk, Nk, weights);
console.log("Optimization Score:", optimizationScore);
```


# Tools
- Calculate keyword difficulty score: https://dataforseo.com/help-center/what-is-keyword-difficulty-and-how-is-it-calculated
- SEO Linter checks html for SEO: https://github.com/nickreese/seo-lint
- SEO Analyzer lib to get SEO score: https://github.com/Bishwas-py/seord
- SEO keyword research tool with Serpapi (autocomplete, related searches, related questions): https://github.com/chukhraiartur/seo-keyword-research-tool
- Google Ads API to get the keyword volume: https://dev.to/hassancs91/how-to-build-your-own-keyword-tool-with-google-ads-api-c-1dec
- POC to get the keyword intersection: https://github.com/tarasowski/trigrams-corpus-analysis
