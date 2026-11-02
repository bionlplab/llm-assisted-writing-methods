# LLM assisted writing deserves empirical evaluation

Xuan Zhong Feng<sup>1</sup>, Yi Lin<sup>1</sup>, Yiye Zhang<sup>1</sup>, Chunhua Weng<sup>2</sup>, Yifan Peng<sup>1,\*</sup>

<sup>1</sup> Department of Population Health Sciences, Weill Cornell Medicine, New York, NY, USA<br>
<sup>2</sup> Department of Biomedical Informatics, Columbia University, New York, NY, USA<br>
<sup>\*</sup> Corresponding author: <yip4002@med.cornell.edu>

## Methods

### Dataset construction

We constructed a corpus of Health Informatics publications from PubMed by selecting records indexed with the MeSH Major Topic “Health Informatics” (MeSH ID D008490). We restricted the retrieval to journal articles and excluded reviews, systematic reviews, meta-analyses, editorials, letters, comments, news items, biographies, errata, and retracted publications. The initial retrieval yielded 86,549 unique records within the study windows. We excluded 6,449 records without abstracts and 10,891 records that were not journal articles, leaving a final analytic sample of 69,209 publications.

To examine temporal shifts associated with the emergence of LLMs, we partitioned the corpus into two periods anchored to the public release of ChatGPT-3.5 ([OpenAI 2022](#ref-openai2022chatgpt)) on November 30, 2022. The pre-LLM period contained publications from January 1, 2015, through November 30, 2022 ($`n=53,575`$), while the post-LLM period contained publications from December 1, 2022, to February 1, 2026 ($`n=15,634`$).

### Identification of LLM-assisted writing

We developed a supervised classifier to identify LLM-assisted scientific writing. A SciBERT model ([Beltagy et al. 2019](#ref-beltagy-etal-2019-scibert)) was fine-tuned on a curated training set of original pre-LLM abstracts and LLM-generated rewrites. The training set consisted of 2,500 original abstracts and 2,500 corresponding GPT-4.1 ([OpenAI 2025](#ref-openai2025gpt41)) generated versions produced using four rewriting strategies: full rewrite (Supplementary Prompt 1), polish (Supplementary Prompt 2), light edit (Supplementary Prompt 3), and human-like rewrite (Supplementary Prompt 4). Original abstracts and their rewritten counterparts were assigned to the same data split to prevent information leakage.

The classifier was calibrated to prioritize precision over recall. For threshold selection, we constructed a balanced labeled dataset of 14,368 abstracts and five pre-LLM negative-control samples of 2,500 abstracts each, excluding the abstracts used to construct the training pairs. At the selected threshold of approximately 0.999876, precision was 0.981, recall was 0.441, and F1 was 0.608 on the labeled threshold-tuning dataset. Across the pre-LLM samples, the mean false-positive rate was 0.00912 and the maximum was 0.010. This conservative threshold was selected because the aim of the study was not to identify all LLM-assisted publications, but rather to define a high-confidence subset of papers with likely LLM-associated writing.

Publications in the post-LLM period with classifier scores above this threshold were assigned to the LLM-assisted group ($`n = 2,673`$). The remaining post-LLM publications were assigned to the LLM-independent group ($`n = 12,961`$). This design allowed us to distinguish broader temporal shifts from additional effects associated with LLM-assisted writing within the post-LLM era.

### Feature extraction

We characterized publications in multiple complementary features because no single one can simultaneously capture semantic content, focal subject matter, and cited knowledge inputs (Supplementary Table S1). These features support different downstream analyses, including semantic concentration, topic composition, and interdisciplinarity.

#### Semantic representation

To capture semantic content across publications, we encoded each title and abstract with SPECTER2 ([Singh et al. 2023](#ref-singh2022scirepeval); [Allen Institute for AI 2023](#ref-allenai2023specter2base)). This model provides dense representations of scientific documents in a shared embedding space. Following the official implementation guidelines, we used the base model together with its proximity adapter to encode each publication into a 768-dimensional vector.

#### Focal topics

We used MeSH descriptors assigned in PubMed to characterize focal topics ([National Library of Medicine 2026](#ref-NLMMeSH2026)). MeSH descriptors and OpenAlex topic labels were mapped to a shared ontology with 31 subtopics under Biological Sciences and Health Sciences. Biological Sciences subtopics followed the Nature Portfolio subject taxonomy, while Health Sciences subtopics were consolidated to capture clinically oriented themes relevant to health informatics. Azure OpenAI GPT-4.1, version 2025-04-14, assigned an ontology subtopic, confidence score, and rationale to each label using the system and user prompts provided in Supplementary Prompts 5 and 6. The mapping file contained 13,540 MeSH descriptors and 2,565 OpenAlex labels. Of these 16,105 labels, 14,765 had confidence scores of at least 0.70 and 1,340 had lower scores. The review flag covered 1,347 labels, including the 1,340 lower-confidence labels and seven higher-confidence labels. These flags identify assignments selected for inspection and do not themselves document completion of manual review.

#### Reference topics

To characterize the topical breadth of cited knowledge, we retrieved each referenced paper’s OpenAlex topic assignment ([Priem et al. 2022](#ref-priem2022openalex)), mapped those labels into the same 31-subtopic ontology, and used the resulting reference-topic profiles to quantify the topical composition of each bibliography and drive the interdisciplinary measures.

#### Language barrier

English remains the dominant language of indexed scientific publishing, including most peer-reviewed journals and international conferences, potentially disadvantaging researchers whose primary language is not English. Here, we investigate whether LLMs help mitigate such language-related barriers. We define a paper *Anglophone* if the first author’s affiliation country appeared in either of two Wikipedia categories, “English is a de jure official language” or “English is a predominant language without de jure designation as official” ([Wikipedia contributors 2026](#ref-wikipediaContributors2026list)).

The countries are: (1) Antigua and Barbuda, (2) Australia, (3) Bahamas, (4) Barbados, (5) Belize, (6) Botswana, (7) Burundi, (8) Cameroon, (9) Canada, (10) Dominica, (11) Eswatini, (12) Fiji, (13) Gambia, (14) Ghana, (15) Grenada, (16) Guyana, (17) India, (18) Ireland, (19) Jamaica, (20) Kenya, (21) Kiribati, (22) Lesotho, (23) Liberia, (24) Malawi, (25) Malta, (26) Marshall Islands, (27) Micronesia, (28) Namibia, (29) Nauru, (30) New Zealand, (31) Nigeria, (32) Pakistan, (33) Palau, (34) Papua New Guinea, (35) Philippines, (36) Rwanda, (37) Saint Kitts and Nevis, (38) Saint Lucia, (39) Saint Vincent and the Grenadines, (40) Samoa, (41) Seychelles, (42) Sierra Leone, (43) Singapore, (44) Solomon Islands, (45) South Africa, (46) South Sudan, (47) Sudan, (48) Tanzania, (49) Tonga, (50) Trinidad and Tobago, (51) Tuvalu, (52) Uganda, (53) United Kingdom, (54) United States, (55) Vanuatu, (56) Zambia, (57) Zimbabwe.

#### Global access gap

Scholars in low- and middle-income economies (LMICs) have historically faced barriers to scientific publishing, including limited access to literature and resource constraints. We examine whether LLM tools are associated with improvements in research output from these settings. We define a paper as *LMIC* if the last author is affiliated with institutions in low- and middle-income economies based on World Bank Country Classifications.

Low-income economies include: (1) Afghanistan, (2) Burkina Faso, (3) Burundi, (4) Central African Republic, (5) Chad, (6) Congo, Dem. Rep., (7) Eritrea, (8) Gambia, The, (9) Guinea-Bissau, (10) Korea, Dem. People’s Rep., (11) Liberia, (12) Madagascar, (13) Malawi, (14) Mali, (15) Mozambique, (16) Niger, (17) Rwanda, (18) Sierra Leone, (19) Somalia, Fed. Rep., (20) South Sudan, (21) Sudan, (22) Syrian Arab Republic, (23) Togo, (24) Uganda, (25) Yemen, Rep.

Lower middle income economies include: (1) Angola, (2) Bangladesh, (3) Benin, (4) Bhutan, (5) Bolivia, (6) Cambodia, (7) Cameroon, (8) Comoros, (9) Congo, Rep., (10) Côte d’Ivoire, (11) Djibouti, (12) Egypt, Arab Rep., (13) Eswatini, (14) Ghana, (15) Guinea, (16) Haiti, (17) Honduras, (18) India, (19) Jordan, (20) Kenya, (21) Kiribati, (22) Kyrgyz Republic, (23) Lao PDR, (24) Lebanon, (25) Lesotho, (26) Mauritania, (27) Micronesia, Fed. Sts., (28) Morocco, (29) Myanmar, (30) Namibia, (31) Nepal, (32) Nicaragua, (33) Nigeria, (34) Pakistan, (35) Papua New Guinea, (36) Philippines, (37) São Tomé and Príncipe, (38) Senegal, (39) Solomon Islands, (40) Sri Lanka, (41) Tajikistan, (42) Tanzania, (43) Timor-Leste, (44) Tunisia, (45) Uzbekistan, (46) Vanuatu, (47) Vietnam, (48) West Bank and Gaza, (49) Zambia, (50) Zimbabwe.

#### International collaboration

To characterize collaboration structure, we defined *international collaboration* as papers with author affiliations spanning more than one continent.

### Outcome measures

We evaluated publications using bibliometric, semantic, topical and authorship-related measures. In addition to standard variables such as raw cited-by count, journal impact factor, and number of references, we introduced several derived measures for this study. These measures were designed to make papers comparable across publication windows, summarize the topical structure of their cited and focal knowledge, quantify semantic concentration in embedding space, and characterize authorship representation across country contexts.

#### Distance to topic centroid

We computed the Euclidean distance between a paper’s embedding and the centroid of its focal topic in the whole cohort (pre-LLM, LLM-independent, LLM-assisted).

#### Reference-list length

Reference-list length was measured using the number of references recorded in the saved OpenAlex-derived per-paper data. It measures publications cited by a paper, while cumulative citations measure subsequent publications citing that paper.

#### Rao-Stirling diversity of reference topics

To measure the breadth and diversity of cited knowledge, we computed *Rao-Stirling diversity* of each paper’s reference topic profile, defined as $`D_p = \sum_i \sum_j p_i p_j d_{ij}`$, where $`p_i`$ is the proportion of paper $`p`$’s references assigned to topic $`i`$, and $`d_{ij}`$ is the disparity between topics $`i`$ and $`j`$. We set $`d_{ij}=0`$ for identical topics, $`d_{ij}=0.5`$ for different subtopics within the same major ontology domain, and $`d_{ij}=1`$ for subtopics from different major domains.

#### Focal-reference Rao-Stirling distance

Similarly, to measure how far a paper’s cited knowledge extends from its own focal topic profile, we computed *focal-reference Rao-Stirling distance*, defined as $`D_p^{FR} = \sum_i \sum_j f_i r_j d_{ij}`$, where $`f_i`$ is the proportion of paper $`p`$’s focal topics assigned to topic $`i`$, $`r_j`$ is the proportion of paper $`p`$’s references assigned to topic $`j`$, and $`d_{ij}`$ is the disparity between focal and reference topics. We used the same ontology distance definition: $`d_{ij}=0`$ for identical topics, $`d_{ij}=0.5`$ for different subtopics within the same major ontology domain, and $`d_{ij}=1`$ for subtopics from different major domains.

#### Cumulative citation acquisition

To account for differences in citation exposure time across publication dates, we retrieved exact citing works from OpenAlex and counted non-negative-lag citation events by the citing work’s publication date. For each paper $`p`$ and post-publication month $`m`$, we computed cumulative citations as $`C_p(m)`$, the number of citations received from publication through month $`m`$. A paper contributed to month $`m`$ only if that month was observable by the analysis date. The primary visibility analysis used cumulative citations through 24 months after publication; citations occurring before the target paper’s publication date were excluded. Cumulative citation comparisons at 12, 24, and 36 months are reported in Supplementary Table S2. Each window includes only papers with sufficient observable follow-up. These comparisons use different eligible subsets and should not be read as changes within a fixed sample. No LLM-assisted papers had sufficient follow-up for a 48-month comparison.

#### Journal impact factor

We used the OpenAlex source-level field `summary_stats.2yr_mean_citedness` as the journal impact factor metric in this study. Available values were summarized within the validated cohort.

#### Journal access model

We used OpenAlex journal-level access metadata to classify fully open-access journals, including fee-free diamond journals, as Gold Open Access and other journals as subscription/hybrid. Open-access articles in hybrid journals remained in the subscription/hybrid category. Papers without usable metadata and records assigned to non-journal sources were excluded. Supplementary Table S3 reports the proportions among papers with identifiable status. Sensitivity analyses used Directory of Open Access Journals membership and known transitions to fully open access, excluding papers published in the transition year.

### Statistical analysis

Because LLM-assisted papers were necessarily published in the post-LLM period, we separated the analysis into two contrasts: LLM-assisted versus post-era LLM-independent papers, and LLM-assisted papers versus pre-LLM papers. This design allowed us to distinguish patterns associated with LLM-assisted writing from broader temporal differences between the pre- and post-LLM periods. For outcomes shown with statistical annotations, we used two-sided Mann–Whitney U tests ([Mann and Whitney 1947](#ref-mann1947test)). Journal-access distributions were compared using chi-square tests, with Yates correction for pairwise two-by-two comparisons. We adjusted the three pairwise comparisons using the Benjamini–Hochberg procedure. The cumulative citation table reports unadjusted two-sided Mann–Whitney U test values alongside the corresponding adjusted values.

## References

- <a id="ref-allenai2023specter2base"></a>Allen Institute for AI. 2023. *allenai/specter2\_base*. <https://huggingface.co/allenai/specter2_base>
- <a id="ref-beltagy-etal-2019-scibert"></a>Beltagy, Iz, Kyle Lo, and Arman Cohan. 2019. “SciBERT: A Pretrained Language Model for Scientific Text.” In *Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing and the 9th International Joint Conference on Natural Language Processing (EMNLP-IJCNLP)*, edited by Kentaro Inui, Jing Jiang, Vincent Ng, and Xiaojun Wan. Association for Computational Linguistics. <https://doi.org/10.18653/v1/D19-1371>
- <a id="ref-mann1947test"></a>Mann, Henry B., and Donald R. Whitney. 1947. “On a Test of Whether One of Two Random Variables Is Stochastically Larger Than the Other.” *Ann. Math. Stat.* 18 (1): 50–60. <https://doi.org/10.1214/aoms/1177730491>
- <a id="ref-NLMMeSH2026"></a>National Library of Medicine. 2026. *Medical Subject Headings*. U.S. National Institutes of Health. <https://www.nlm.nih.gov/mesh/>
- <a id="ref-openai2022chatgpt"></a>OpenAI. 2022. *Introducing ChatGPT*. <https://openai.com/index/chatgpt/>
- <a id="ref-openai2025gpt41"></a>OpenAI. 2025. *Introducing GPT-4.1 in the API*. OpenAI Blog. <https://openai.com/index/gpt-4-1/>
- <a id="ref-priem2022openalex"></a>Priem, Jason, Heather Piwowar, and Richard Orr. 2022. *OpenAlex: A Fully-Open Index of Scholarly Works, Authors, Venues, Institutions, and Concepts*. Preprint at <https://arxiv.org/abs/2205.01833>
- <a id="ref-singh2022scirepeval"></a>Singh, Amanpreet, Mike D’Arcy, Arman Cohan, Doug Downey, and Sergey Feldman. 2023. “SciRepEval: A Multi-Format Benchmark for Scientific Document Representations.” In *Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing*, edited by Houda Bouamor, Juan Pino, and Kalika Bali. Association for Computational Linguistics. <https://doi.org/10.18653/v1/2023.emnlp-main.338>
- <a id="ref-wikipediaContributors2026list"></a>Wikipedia contributors. 2026. *List of Countries and Territories Where English Is an Official Language*. Wikimedia Foundation, Inc. <https://en.wikipedia.org/wiki/List_of_countries_and_territories_where_English_is_an_official_language>