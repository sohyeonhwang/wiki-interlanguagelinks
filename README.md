# Wikipedia rule interlanguage links
Code and notes for grabbing and handling interlanguage links for "rule" pages on Wikipedia. A lot of the code is probably not very elegant/efficient because most of it is from me trying to figure out how to handle the data in the first place (aka the code is not optimized) and some of it is some of the first real coding I did in grad school, please feel free to learn from and improve upon my errors :)

**Note** some of the input/output + file paths might not work because I pulled this together retroactively to share (so that people don't have to go through the million other non-relevant and potentially very confusing files).

There are a couple of files here:

* In the `rule_lists` folder are lists of rules I manually compiled on the English, French, German, Japanese, and Spanish language editions. I basically had a flowchart for deciding if a page was a rule or not. I started by grabbing all pages that were on policy/guideline categories/lists for a language edition (you can find some messy documentation of this process copy and pasted into a subsection below). The code to do a lot of this are the `shortcuts_get_[lang].py` files.

* Some of the code relies on Brian Keegan's [`wikifunctions` code](https://github.com/brianckeegan/wikifunctions/). I have a local, much shorter version of this that I use for a few notebooks, in `wikifunctions_sohyeon.py` that also has a custom function. Some of the scripts probably just rely on Brian's .py file though.

*  If you want a more comprehensive list of "rules": `nm4.ipynb` contains code for grabbing all the pages from namespace 4 (the Wikipedia Project namespace, AKA where all rule pages are located) and then their interlanguage links, and filtering it. You can decide which language editions you would like to do this for. However, this includes potentially a lot of non-rule Wikipedia Project pages. Note that getting all of the interlanguage links can take some time. I recommend parallelizing the code and running it from terminal as a Python script rather than using a notebook to run it. Because it takes long, I'm sharing my outputs in the `nm` folder for the top 10 language editions (by count of active editors). I recently did this (6/15/2022), so it should be fairly up to date.

* `link_dates` contains code for identifying the dates that interlanguage links were made from revision histories (`ill_get_dates.ipynb`). This isn't perfect, however, so there's a notebook where I figured out which ones we couldn't retrieve dates for (`ill_find_missing.ipynb`). Then, an undergraduate RA manually went in and filled in that missing data, resulting in the `compiled_[lang].xlsx` files (I believe that the manual `compiled_[lang].xlsx` files and the automatic `interlanguagelinks_dates_[lang].json` files still need to be merged/resolved, but can't remember). The notebooks are both examples of code that would probably be better being cleaned and converted into scripts to run, rather than notebooks, but I was troubleshooting a lot and trying to figure out how to do all this so the notebooks were handy at the time...

* Note: interlanguage links were manually added at first, before WikiData started handling it (and the manually added links were mostly removed). This means that ILLs were not always symmetric links and in some cases, still are not if there are residual manually added links.

* In order to avoid the complexity of the interlanguage link adding, you could instead just figure out what pages are currently linked (fairly easy peasy, just grab the interlanguage links) and then find it's first edit to identify the date of creation (also simple). However, thinking about the creation of a page and when two pages were linked have different implications for what dynamics one is considering.

## Sohyeon's process for manually constructing lists of rules.

I did this in early 2020 if I remember correctly. The basic idea was I (1) grabbed all pages that were listed on the policy and guideline pages (e.g., "List of policies", or their equivalent, as language editions don't necessarily organize their rules the same way); (2) manually cross-checked the overall list this with what pages were under the policy/guideline/etc. *category*; (3) manually evaluated the pages based on the criteria in the flowchart below:

![This image contains a flowchart for the logic Sohyeon used to determine if a page in the initial scraped and cross-checked set of potential rule pages should be kept as a rule page. If a page has a direct indicator that it's a policy / guideline, then we keep it. If it is in the Wikipedia namespace (aka, namespace 4) and we find that it is normative in nature --- that is, it directs people how they should act and operate --- we keep it (however, if there are indicators that the page is not widely shared, it is rejected). If it is not in the Wikipedia namespace but it is under a WikiProject/Portal and is normative in nature, we keep it. Next, if it is a draft, proposal, user page, or tutorial, we reject it.](202104__ruleflowchart.png "Potential rule page evaluation flow chart")

To grab all the pages that were listed on policy and guideline pages, this requires first examining the way that a language edition has decided to organize it's rules. Most of the time this is a Policy page and a Guidelines page (list of policies, list of guidelines). Then I looked at the HTML of the page and scraped all the relevant potential rule links as well as the shorthand for referring to these links (because I am interested in the use of those shorthands, but you can exclude this part if desired obviously). Those are the `shortcuts_get_[lang].py` files, and after some handling of the outputs of those scripts, the results are the rule_list .tsv files.

Here is a crude copy-and-pasted list of notes/documentation I had for English, French, German, Spanish, Japanese: 

----
**ENGLISH**<br />
List constructed from:
* https://en.wikipedia.org/wiki/Wikipedia:List_of_policies
* https://en.wikipedia.org/wiki/Wikipedia:List_of_guidelines

Cross-checked results with category pages:
https://en.wikipedia.org/wiki/Category:Wikipedia_policies
*Issues (Resolved)*
* https://en.wikipedia.org/wiki/Wikipedia:Non-free_content --> actually in guidelines
* Wikipedia:Signatures --> actually in guidelines

https://en.wikipedia.org/wiki/Category:Wikipedia_guidelines
*Issues (resolved)*
* https://en.wikipedia.org/wiki/Wikipedia:Content_assessment --> is "Version 1.0 Editorial Team assessment"

* added in missed pages of guidelines from cross-check:
  * https://en.wikipedia.org/wiki/Wikipedia:Scientific_citation_guidelines
  * https://en.wikipedia.org/wiki/Wikipedia:Artist%27s_impressions_of_astronomical_objects
  * https://en.wikipedia.org/wiki/Wikipedia:In_the_news/Recurring_items
  * https://en.wikipedia.org/wiki/Wikipedia:Identifying_reliable_sources_(medicine)
  * https://en.wikipedia.org/wiki/Wikipedia:Indic_transliteration
  * https://en.wikipedia.org/wiki/Wikipedia:Non-free_use_rationale_guideline
  * https://en.wikipedia.org/wiki/Wikipedia:Public_domain
  * ... and so on. See file for all of them. There are about 30-35.

----
**SPANISH**<br />
List constructed from:
* https://es.wikipedia.org/wiki/Categor%C3%ADa:Wikipedia:Pol%C3%ADticas
* https://es.wikipedia.org/wiki/Categor%C3%ADa:Wikipedia:Convenciones

Cross-checked results with shortcuts list: https://es.wikipedia.org/wiki/Ayuda:Lista_de_atajos

Added Manual of Style Links manually:
* https://es.wikipedia.org/wiki/Categor%C3%ADa:Wikipedia:Manual_de_estilo (multiple)

----
**FRENCH**<br />
List constructed from:
* https://fr.wikipedia.org/wiki/Cat%C3%A9gorie:Wikip%C3%A9dia:R%C3%A8gle
* https://fr.wikipedia.org/wiki/Cat%C3%A9gorie:Wikip%C3%A9dia:Recommandation

Cross-checked results with shortcuts list: https://fr.wikipedia.org/wiki/Aide:Raccourcis_Wikip%C3%A9dia

Added in 5 principles to the policies list via manual inclusion:
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Principes_fondateurs
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:R%C3%A8gles_de_savoir-vivre
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Interpr%C3%A9tation_cr%C3%A9ative_des_r%C3%A8gles
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Droit_d%27auteur
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Neutralit%C3%A9_de_point_de_vue
* https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Wikip%C3%A9dia_est_une_encyclop%C3%A9die

----
**JAPANESE**<br />
List constructed from:
* [Wikipedia:方針とガイドラインの一覧
 aka Wikipedia: List of policies and guidelines
, which is more thorough than their separate policy + guideline pages](https://ja.wikipedia.org/wiki/Wikipedia:%E6%96%B9%E9%87%9D%E3%81%A8%E3%82%AC%E3%82%A4%E3%83%89%E3%83%A9%E3%82%A4%E3%83%B3%E3%81%AE%E4%B8%80%E8%A6%A7)

Cross-checked results with Category pages.

Added manually:
* https://ja.wikipedia.org/wiki/Wikipedia:%E4%BA%94%E6%9C%AC%E3%81%AE%E6%9F%B1
* https://ja.wikipedia.org/wiki/Wikipedia:%E8%A8%98%E4%BA%8B%E5%90%8D%E3%81%AE%E4%BB%98%E3%81%91%E6%96%B9/%E6%97%A5%E6%9C%AC%E3%81%AE%E7%9A%87%E6%97%8F
* https://ja.wikipedia.org/wiki/Wikipedia:%E8%91%97%E4%BD%9C%E6%A8%A9/20080630%E8%BF%84
* https://ja.wikipedia.org/wiki/Wikipedia:%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E4%BD%9C%E6%88%90%E8%80%85
* https://ja.wikipedia.org/wiki/Wikipedia:IP%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF%E9%81%A9%E7%94%A8%E9%99%A4%E5%A4%96

----
**GERMAN**<br />
6/22/2022: Honestly I forgot to document this back in the day. But I had followed th esame process in essence, but it was different as rules on German are not in the same Policy/Guidelines categorization, but just "Richtlinien". I systematically went through all the links from the Richtlinien page, cross-checked, and compiled a list of rules in a similar manner.

There is no script because I did this one entirely manually.