# Overview 
In languages such as European Portuguese, verbs are conjugated for all persons, meaning each person is marked with a different ending or suffix in the conjugation. This allows the speaker to refer to the subject solely through the verb form. Portuguese is a null-subject or pro-drop language, meaning it allows the omission of pronouns, especially subject pronouns. The subject pronoun can be expressed in two ways: either through an explicitly stated pronoun, as in the sentence "Eu bebo café" (English: "I drink coffee"), where "Eu" is the personal pronoun for the first-person singular, or by being linked to the verb form, as in the sentence "Bebo café", where the pronoun is omitted. Despite the absence of the explicit subject pronoun, the suffix "-o" in "bebo" still indicates that the subject is first person singular. 


# Why I chose this phenomenon
I chose to focus on this phenomenon because null subjects present a remarkable challenge within the framework of LFG. The challenge lies in structuring the grammar in such a manner that, despite the subject's absence from the sentence, it remains accurately represented in the f-structure. In order to accurately model null subject languages, it is important to address this issue when using them.


# Implementation approach/design 
My implementation approach is derived from a well-established approach in the literature for handling null subject languages, such as Italian, Spanish, and Finnish, within the LFG framework. Ida Toivonen (2020) proposes analyzing this phenomenon through pronoun incorporation. This theory proposes that verb morphology, specifically the conjugational affix, such as the "-o" in the verb "bebo," serves a dual purpose: it serves as both an agreement marker and an incorporated pronoun. When the subject pronoun is explicitly stated, the verb morphology functions as an agreement marker. Nonetheless, in the absence of the subject pronoun, the verb morphology assumes the role of the pronoun. This dual functionality is reflected in the lexical entries, where the suffix "-o" has two possible entries: one with a PRED value, where it acts as an incorporated pronoun, and one without, serving solely as an agreement marker. This approach ensures the correct representation of omitted subject pronouns in the f-structure while maintaining syntactic integrity. A more detailed description of this phenomenon and its treatment within LFG is provided in my LFG paper. The research findings from that paper are the basis of my implementation, with Toivonen's analysis of Italian, Spanish, and Finnish serving as a reference point.








Toivonen's approach makes sense and can be easily implemented in the LFG. However, the implementation in XLE  is somewhat different. Theoretically, I should have added an optional PRO feature to my lexicon entries and split my verbs into morphemes. This would have made it possible to treat the optionality of the PRED value as part of the conjugation affixes of the verbs. However, I did not implement this method, firstly because I was not sure if it was technically feasible, and secondly because it would have been too complex for my level of knowledge.

Instead, I chose a different approach. I followed the implementation of imperatives and inserted the following rule in the sentence part:


    S --> { NP: (^ SUBJ) = !
	        (! CASE) = nom
     
      | e: (^ SUBJ PRED) = 'pro'			"allow null-subject as well"	
      	   @(OT-MARK NullSubj) }		"prefer null-subject"
          
      	VP: (^ TNS-ASP TENSE);		
 	    (PERIOD: @(OT-MARK Punct)).


In this way, in addition to the NP, it is also possible to use a null subject.
To distinguish this rule from the rule for imperatives, I have added an additional restriction for imperatives that the mode must be imperative.

One of the biggest challenges in implementing the grammar was creating a whole new language. This posed particular challenges in the area of agreement. This affected not only the verb agreement, but also prepositions, adjectives and possessive pronouns, whereby I used a similar approach to the verb agreement in each case.
A remaining problem relates to the null subject. According to the literature, subjects are omitted when they can be implied from the context and are only explicitly used as independent pronouns when contrast is intended. This grammar, however, contains no such restrictions, it simply allows both options without further restrictions. However, I have used at least an OT marker to prefer the option without a pronoun, i.e. the null subject, as this is the preferred option.
Another challenge refers to the position of adjectives in the sentence. In EP, adjectives can be placed both before and after the noun. However, the position depends on the type of adjective, for example, whether it expresses a color or a quality. These restrictions have not been taken into account in the grammar either. However, I have at least inserted an OT marker here too, which favors the standard position (after the noun).
Another challenge was to ensure that names could only appear in combination with articles. To do this, I proceeded in a similar way to the possessive pronouns and added a rule to the NP and restricted the rule to the ntype “name”. I also had to ensure that the combination of a preposition and a name works correctly. To do this, I implemented an additional option in the NP rule and updated the PP rule so that it is possible for a name to appear directly after a preposition.



# Overview of the OT marks used

Here are the OT marks I have used in my grammar:

+cat: I used this OT marker to prefer a certain category, especially when a word can function e.g. as both a verb and a noun. This prioritizes the more likely interpretation.

+NullSubj: This OT marker was used to prefer sentences without an explicit subject pronoun, so that the subject is expressed implicitly (null subject). This is standard usage in languages such as EP. 

+Advattach: I have used this OT marker to prefer the position of an adverb. As it can come before or after the verb in a sentence, I have preferred the position after the verb, as this is the more common position.

+APattach: With this OT marker, the position of adjectives after the noun is preferred, as this corresponds to the standard position in EP.

In addition, I have introduced a restriction in the NP rule to ensure that a pronoun is a possessive pronoun. This restriction ensures that a possessive pronoun is always accompanied by a determiner.

# Submitted Files

Grammar: grammar_GD_jennifer_coutinho_goncalves.lfg
Testsuite: testsuite_GD_jennifer_coutinho_goncalves.lfg
Templates: common.templates.lfg
Glossing: glossing_GD_jennifer_coutinho_goncalves.pdf
LFG Paper: paper_LFG_jennifer_coutinho_goncalves.pdf

# GitHub Link: https://github.com/jennifercoutinhogoncalves/finalproject_grammardevelopment

# References 

Huot, Marta. "Ana Tavares, Português XXI. Livro do Aluno 1." Artifara 7 (2007): 5-9.

Martins, Maria Teresa Hundertmark-Santos. Portugiesische Grammatik. Walter de Gruyter, 2012.

Toivonen, Ida. "Pronoun incorporation." Handbook of Lexical Functional Grammar (2023): 563-601.


