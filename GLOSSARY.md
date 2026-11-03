# Coffee Atlas

Established domain language from [roadmap #1](https://github.com/BrandonTing/coffee_atlas/issues/1) and issues [#3](https://github.com/BrandonTing/coffee_atlas/issues/3) and [#4](https://github.com/BrandonTing/coffee_atlas/issues/4).

## Language

**Bean**:
A coffee identity described by available information such as origin, processing, roast, and roaster. Discovery can discuss a bean before it has a confirmed collection record.

**Discovery**:
The conversation that begins with a bean description or packaging photo and develops an explanation and proposed recipe.

**Proposed Recipe**:
An editable brewing plan with parameters, reasoning, and uncertainty. Its planned parameters are distinct from what the user actually brews.

**Brew Session**:
An actual brewing event, with user-reported parameters where known. Missing actual parameters remain unknown.

**Tasting Feedback**:
The user's reported sensory impressions of an actual brew.

**AI Hypothesis**:
An AI-proposed explanation or possible adjustment, distinct from a user observation or established fact.

**Provenance**:
The attribution of information to sources such as label extraction, user input, or AI inference, including an explicit unknown where applicable.

**Structured Draft**:
An editable extraction prepared on explicit Save intent for review and correction before confirmation. It is not yet a confirmed saved record.

**Confirmed Structured Record**:
Structured information explicitly reviewed and confirmed by the user for persistence. Confirmed structured records are the source of truth for saved facts and comparisons.

**Collection**:
All confirmed beans, independent of Brew Journey activity.

**Brew Journey**:
An experiment that can be explicitly active or historical. Its identity is distinct from Bean identity; a bean may support multiple experiment goals.
