# Literature Review: Computational Modeling of Pancreatic Islet Insulin Secretion and Glucose Response Using Biophysical Ionic Models

**Focus:** Beta-cell ionic/oscillator models (Chay-Keizer lineage) | Islet-scale coupling (monodomain & coupled-oscillator frameworks) | Computational insulin secretion dynamics

---

## 1. Introduction and Scope

The pancreatic islet of Langerhans is a multicellular endocrine micro-organ whose primary function — glucose-stimulated insulin secretion (GSIS) — emerges from the collective electrophysiological, metabolic, and paracrine dynamics of its constituent cells. At the heart of this system are the β-cells, which couple intracellular glucose metabolism to membrane electrical activity and Ca²⁺ signaling, ultimately triggering insulin exocytosis in a characteristic pulsatile fashion. Modeling these phenomena computationally has proceeded along two parallel tracks: (1) progressively refined **single-cell biophysical ionic models** that capture the molecular mechanisms of bursting and oscillatory behavior; and (2) **multicellular islet-scale frameworks** that address how hundreds of electrically coupled, heterogeneous cells synchronize to produce coherent secretory outputs.

This review surveys the computational literature at the intersection of these tracks. It is organized as follows: Section 2 covers the foundational and descendant single-cell ionic models; Section 3 addresses the Dual Oscillator and Integrated Oscillator Model lineages; Section 4 reviews monodomain and PDE-based spatial frameworks; Section 5 covers coupled-oscillator networks and heterogeneity; Section 6 addresses organoid and multi-physics models; Section 7 identifies open problems and emerging directions.

---

## 2. Foundational Single-Cell Biophysical Ionic Models

### 2.1 The Chay-Keizer Model (1983) — Origin of the Lineage

The seminal work of **Chay and Keizer (1983)** (*Biophys. J.* 42:181–190) introduced the first quantitative biophysical model of electrical bursting in pancreatic β-cells. Constructed in close analogy to the Hodgkin-Huxley formalism for nerve excitation, it incorporated voltage-gated Ca²⁺ channels, a Ca²⁺-dependent K⁺ channel, and a delayed-rectifier K⁺ channel, with cytosolic Ca²⁺ acting as the slow variable driving burst termination. The model produced periodic bursting action potentials — an "active phase" of spiking followed by a "silent phase" — corresponding to experimentally observed oscillatory electrical activity in intact islets. The key mechanistic insight was that the accumulation of intracellular Ca²⁺ during the active phase activates K(Ca) channels, hyperpolarizing the membrane and initiating the silent phase. This fast-slow dynamical structure, first analyzed formally by **Rinzel (1985, 1987)**, became the interpretive template for all subsequent models.

The fast-slow decomposition of the Chay-Keizer model (reviewed in **Bertram et al., 2023**, *Math. Biosci.* 365:109085) reveals bistability in the fast subsystem (voltage + gating variables) with Ca²⁺ as the bifurcation parameter. Bursting arises because the slow Ca²⁺ variable drives the fast subsystem periodically through a region of bistability, producing the familiar plateau-burst pattern. This framework proved widely applicable, and numerous variants appeared through the 1980s and 1990s incorporating additional currents (e.g., ATP-sensitive K⁺ channels, K(ATP)), endoplasmic reticulum (ER) Ca²⁺ handling, and mitochondrial metabolism.

### 2.2 K(ATP) Channel Extensions and the Phantom Burster

**Keizer and Magnus (1989)** extended the Chay-Keizer framework by replacing K(Ca)-dependent bursting with K(ATP)-dependent bursting, incorporating the then-newly discovered ATP-sensitive K⁺ channel whose conductance decreases with rising cytosolic ATP/ADP ratio. This shift linked electrical activity more directly to glucose metabolism and became a dominant paradigm through the 1990s. **Smolen and Keizer (1992)** further investigated conditions for bursting with this mechanism.

The issue of "phantom bursting" — where the slow variable need not be identifiable with a single ionic current — was formalized by **Bertram, Butte, Kiemel, and Sherman (1995)** (*Bull. Math. Biol.*). They showed that models with multiple slow processes can exhibit bursting over a wide range of timescales without a single "pacemaker" variable, broadening interpretive possibilities significantly. This concept was revisited by **Fazli, Vo, and Bertram (2020)** (*J. Theor. Biol.* 501:110346), who argued that phantom bursting mechanisms may underlie electrical bursting in isolated single β-cells, where irregular (rather than regular) bursting is typical.

### 2.3 Detailed Conductance Models: Cha-Noma and Related

**Cha, Nakamura, Himeno et al. (2011)** (*J. Gen. Physiol.* 138:21–37) introduced a more comprehensive ionic model incorporating detailed descriptions of eight distinct ionic currents — including L-type and T-type Ca²⁺ channels, delayed rectifier and A-type K⁺ currents, KATP, and ER Ca²⁺ fluxes — to simulate glucose-dependent responses in mouse β-cells. This "Cha-Noma" model has been widely adopted for multicellular network studies (see Section 5) because its conductance parameters are physiologically grounded and produce realistic Ca²⁺ waveforms.

**Félix-Martínez and Godínez-Fernández (2014)** (*Islets* 6:e949195) provided a systematic review of single-cell β-cell electrical models, cataloguing their mathematical structure, ion channel composition, and physiological predictions, offering a useful comparative reference.

**Marinelli et al. (2022)** (*Biophys. J.* 121:1449–1464) demonstrated that oscillations in K(ATP) conductance — driven by metabolic dynamics — can serve as the primary driver of slow Ca²⁺ oscillations, providing a clear mechanistic link between the metabolic and electrical subsystems within the IOM framework (see Section 3.2).

### 2.4 Human β-Cell Models

Most early ionic models were parameterized for mouse β-cells. Human β-cells differ in their ion channel complement, islet architecture (greater α-cell proportion, different gap-junction properties), and their tendency toward fast rather than slow Ca²⁺ oscillations. **Riz et al. (2014)** developed a human-specific conductance model. More recently, **Dwulet, Briggs, and Benninger (2021)** (*PLoS Comput. Biol.* 17:e1008948) coupled human β-cell models in a multicellular setting and showed that small subpopulations of "hub" cells do not dominate islet oscillatory dynamics via gap junctions alone, highlighting species-specific differences. The comparative merits of Cha-Noma (mouse) vs. Riz (human) models in capturing β-cell network heterogeneity were assessed in a 2025 preprint (**bioRxiv 2025.10.26**), finding that neither model as published adequately captures the observed degree of asynchrony in intact networks without additional heterogeneity tuning.

---

## 3. Oscillator Models: From Dual Oscillator to Integrated Oscillator

### 3.1 The Dual Oscillator Model (DOM)

By the early 2000s, experimental evidence had accumulated for two distinct timescales of oscillation in β-cells and islets: "fast" bursting with periods of tens of seconds, and "slow" oscillations with periods of several minutes. **Bertram, Satin, Pedersen, Luciani, and Sherman (2007)** (*Biophys. J.* 92:1544–1555; *Am. J. Physiol.* 293:E890–E900) unified these observations in the **Dual Oscillator Model (DOM)**, in which fast electrical oscillations arise from Ca²⁺ feedback onto K⁺ channels (the classical Chay-Keizer mechanism), while slow oscillations are driven by intrinsic oscillations in glycolysis — specifically, by oscillatory activity of phosphofructokinase (PFK).

The DOM predicts "compound bursting" — slow episodes of fast bursts — as a hybrid of these two mechanisms, a prediction borne out experimentally. Crucially, a glycolytic metabolite, fructose-1,6-bisphosphate (FBP), was predicted to show a sawtooth pattern synchronized with slow bursting, a prediction confirmed by **Merrins et al. (2016)** (*Biophys. J.*) using metabolic imaging.

**McKenna, Dhumpa, Mukhitov, Roper, and Bertram (2016)** (*PLoS Comput. Biol.* 12:e1005143) showed computationally and experimentally that sinusoidal glucose oscillations can "recruit" non-oscillating islets — those in which the glycolytic oscillator is latent — to produce synchronized slow oscillations, providing direct experimental support for the DOM's central hypothesis that an endogenous glycolytic oscillator drives slow pulsatile insulin secretion.

### 3.2 The Integrated Oscillator Model (IOM)

The DOM treated the Ca²⁺ and glycolytic oscillators as relatively independent. The **Integrated Oscillator Model (IOM)**, developed by **Marinelli, Fletcher, Sherman, Satin, and Bertram (2021–2022)** (*Front. Physiol.* 12:781581; *Biophys. J.* 121:692–704; *Biophys. J.* 121:1449–1464), incorporates bidirectional coupling between the two oscillators: Ca²⁺ feeds back onto glycolysis (via Ca²⁺ activation of mitochondrial dehydrogenases and its effect on the FBP/ATP relationship), while glycolytic outputs modulate electrical activity through K(ATP) channel closure.

The key conceptual advance of the IOM is that the relationship between Ca²⁺ and metabolic oscillations is **symbiotic**: in some parameter regimes, electrical oscillations drive metabolic ones; in others, glycolytic oscillations drive Ca²⁺ and electrical dynamics; and in still others, neither subsystem is a simple "master" of the other. **Watts, Fendler, Merrins, Satin, Bertram, and Sherman (2014)** (*SIAM J. Appl. Dyn. Syst.* 13:683–703) posed this as an open question ("Calcium and Metabolic Oscillations in Pancreatic Islets: Who's Driving the Bus?") that the IOM was designed to resolve.

**Marinelli, Vo, Gerardo-Giorda, and Bertram (2018)** (*J. Theor. Biol.* 454:310–319) analyzed transitions between bursting modes in the IOM using fast-slow analysis, showing that the dominant oscillation mechanism can switch as a function of glucose or other parameters — a phenomenon called "regime change" with experimental correlates. **McKenna and Bertram (2018)** (*J. Theor. Biol.* 457:152–162) provided a complete fast-slow analysis of the IOM, characterizing the bifurcation structure underlying each bursting mode.

**Bertram, Marinelli, Fletcher, Satin, and Sherman (2023)** (*Math. Biosci.* 365:109085) provided a comprehensive retrospective and deconstruction of the IOM on the 40th anniversary of the original Chay-Keizer model, tracing the incremental additions of components (ER Ca²⁺, mitochondrial respiration, glycolysis, Ca²⁺-metabolism coupling) and the experimental interactions that motivated each. This is now the definitive review of the Chay-Keizer lineage through to the IOM. Related work by **Fletcher, Thompson, Liu, Bertram, Satin, and Sherman (2023)** (*Am. J. Physiol.* 324:E477–E487) further addressed the relative contributions of Ca²⁺ entry vs. ER Ca²⁺ release in governing oscillation type.

**Corradi, Thompson, Fletcher, Bertram, Sherman, and Satin (2023)** (*J. Physiol.* 601:5655–5667) provided experimental support for K(ATP) channel regulation by mitochondrial ATP production as a key factor in slow oscillations, anchoring the IOM's metabolic sub-model in direct electrophysiology.

### 3.3 Pulsatile Insulin Secretion and the DOM/IOM

A major motivation for oscillator models is to explain the pulsatile nature of insulin secretion observed in portal blood (period ~5 min) and peripheral blood of mammals including humans. **Bertram, Satin, and Sherman (2018)** (*Diabetes* 67:351–359) synthesized the current state of the DOM/IOM in relation to this phenomenon. **Fletcher, Marinelli, Bertram, Satin, and Sherman (2022)** (*Physiology* 37) argued specifically that pulsatile **basal** insulin secretion — not only glucose-stimulated secretion — is driven by glycolytic oscillations, extending the scope of the IOM.

---

## 4. Monodomain and Spatial Continuum Frameworks

### 4.1 The Monodomain/Supercell Approximation

When β-cells are electrically coupled via gap junctions with high conductance, the islet can be approximated as an isopotential "supercell" — a single compartment with membrane area equal to the sum of all β-cells. **Sherman, Rinzel, and Keizer (1988)** (*Biophys. J.* 54:411–425) showed that, in this limit, stochastic channel fluctuations are suppressed (averaged over a large effective membrane), allowing robust bursting to emerge even in parameter regimes where isolated cells would show irregular or absent bursting. **Sherman and Rinzel (1991)** (*Biophys. J.* 59:547–559, *PNAS* 88:9161–9165) further analyzed synchronization in the infinite-coupling (monodomain) limit, providing the mathematical foundation for the concept that "channel sharing" across many cells is essential for islet-level oscillatory coherence.

This monodomain framework is computationally advantageous because it reduces a network of ODEs to a single-cell ODE system with rescaled membrane capacitance and conductances. It continues to be used as a baseline reference for parameter studies of the IOM and DOM.

### 4.2 PDE-Based Spatial Models and Reaction-Diffusion Frameworks

As an intermediate between the monodomain (fully homogeneous) and fully heterogeneous coupled-cell models, spatially continuous reaction-diffusion PDE frameworks have been employed to study Ca²⁺ wave propagation within islets. **Tsaneva-Atanasova, Zimliki, Bertram, and Sherman (2006)** (*Biophys. J.*) used a diffusion-coupled system to study how diffusion of Ca²⁺ and other small molecules through gap junctions can synchronize oscillations across an islet.

**Benninger, Zhang, Head, Satin, and Piston (2008)** (*Biophys. J.* 95:5048–5061) combined experimental Ca²⁺ imaging with a spatial computational model of a cubic β-cell cluster to demonstrate that gap-junction electrical coupling is necessary for Ca²⁺ wave propagation and oscillation synchronization — and that reducing Cx36 coupling slows wave velocity and reduces synchrony in a quantitatively predictable way. This work established a quantitative standard for spatial islet modeling.

**Gosak, Markovič, Dolenšek et al. (2013–2022)** produced a series of papers using graph-theoretic and continuum models of islet functional networks, incorporating spatial arrangement of heterogeneous cells, to study connectivity, functional network topology, and correlation with experimental Ca²⁺ imaging. In particular, they introduced the concept of "functional connectivity" — correlations in Ca²⁺ activity between cell pairs — as an observable linking computation to experiment.

For organoid applications, **Organoid Microphysiological System** models (e.g., *Science Advances* 2020, aba5515) have used 2D finite element method (FEM) reaction-diffusion models of islet physiology embedded in hydrogel matrices to predict oxygen gradients, insulin secretion profiles under static vs. perfusion conditions, and compare with experimental GSIS assays. These spatial models couple diffusion-reaction equations for O₂ and glucose to simplified phenomenological insulin secretion functions, rather than full ionic models, but represent an important class of organoid-specific computational tool.

---

## 5. Coupled Oscillator Networks: Heterogeneity, Synchronization, and Hub Cells

### 5.1 Gap Junction Coupling and the Emergence of Synchrony

The biophysical case for gap-junction (Cx36) electrical coupling as the primary synchronization mechanism in mouse islets is well established computationally. **Sherman and Rinzel (1991)** showed theoretically that coupling noisy, heterogeneous cells can produce synchronized bursting through "channel sharing." **Benninger et al. (2008)** demonstrated quantitatively that coupling strength governs Ca²⁺ wave velocity and synchronization extent. **Merrins et al. (2011–2016)** provided metabolic imaging confirming synchrony of glycolytic oscillations.

**Dwulet, Briggs, and Benninger (2021)** (*PLoS Comput. Biol.* 17:e1008948) used a network model of coupled Cha-Noma β-cells to test whether small "hub" subpopulations drive islet oscillations via gap junctions, finding that no small subpopulation is necessary — oscillatory dynamics is a collective property of the full network. This challenged simpler "pacemaker" interpretations.

**Peercy and Sherman (2022)** (*J. Biosci.* 47:14) revisited the question of whether oscillations require pacemaker cells using analytical and numerical approaches, reinforcing the collective oscillation framework.

### 5.2 Hub, Leader, and First-Responder Cells

The experimental identification of "hub" β-cells with disproportionate influence on islet-wide activity (**Johnston et al., 2016**, *Cell Metab.* 24:389) prompted a wave of computational investigation. **Lei, Kellard, Hara et al. (2018)** (*Islets* 10:151) used simulations to show that hub cells can maintain oscillations in networks where most cells are otherwise subthreshold. **Dwulet et al. (2021)** contested the functional necessity of hubs via gap junctions alone. The most comprehensive recent analysis is by **Benninger group / eLife (2023)**: **Dwulet, Briggs, and Benninger** (*eLife* 2023, article 83147) used both coupled Cha-Noma and coupled IOM models, finding that **β-cell intrinsic dynamics** — particularly the oscillation duty cycle — rather than gap-junction connectivity structure, determines which cells emerge as functional hubs. This was one of the first uses of a coupled IOM in a multicellular setting.

**Šterk, Dolenšek, Stožer et al. (2023)** further characterized hub cell properties using functional network analysis applied to Ca²⁺ imaging data, demonstrating that cells with high network degree (high connectivity in the functional network) correlate with high oscillation duty cycle.

### 5.3 Heterogeneous Network Models and Multiplex Synchronization

**Marinelli et al. (2021, *Front. Physiol.*)** demonstrated the symbiosis of electrical and metabolic oscillations in a network context, showing that coupled IOM cells can exhibit collective behavior not predictable from single-cell analysis.

**Gosak et al. (2023)** (*Frontiers in Network Physiology*) presented a coupled IOM-based network of human β-cells with realistic spatial arrangement, heterogeneity in glycolytic and electrical parameters, and stochasticity, studied through the formalism of **multiplex networks** — simultaneously analyzing electrical and metabolic "layers" of coupling. They found that electrical coupling alone synchronizes membrane potential, but metabolic coupling further enhances coordination at longer spatial ranges, especially relevant for human islets where Ca²⁺ oscillations are predominantly fast.

**Pré et al. (2023)** (*Phys. Rev. E* 108:054409) used a coupled IOM model to show that adding metabolic coupling to electrical coupling increases the spatial range of functional interactions and improves consistency with experimental functional connectivity data.

**Hoang et al.** used FitzHugh-Nagumo oscillator networks on cubic lattice topologies to investigate the role of hub cells and diversity-induced resonance in the collective oscillatory behavior of β-cell-like networks, providing insight into general dynamical mechanisms that do not depend on the specific ionic model chosen.

**Félix-Martínez (2023, *Islets*)** provided a primer that systematically traces the progression from single-cell models to multicellular coupled models, covering the channel-sharing hypothesis, Ca²⁺ wave propagation, hub cell models, and functional connectivity — an excellent entry point to this literature.

### 5.4 Islet Synchronization Across the Pancreas

At the organ scale, synchronization of the ~1 million islets in a human pancreas remains poorly understood but is required to explain the clear pulsatility of portal and peripheral insulin. **Bertram, Sherman, and Satin (reviewed in *PMC* 3120131)** discussed two candidate mechanisms: neural coupling and paracrine/blood-borne insulin/glucagon feedback. **Bruce, Wei, Leng, Roper, and Bertram (2022)** (*Am. J. Physiol.* 323:E492–E502) studied coordination of islet rhythmic activity by delayed negative feedback mechanisms. **Adablah, Vinson, Roper, and Bertram (2019)** (*PLoS One* 14:e0211832) modeled synchronization of islets by muscarinic agonist pulse trains, suggesting that ganglionic pulsatile stimulation could entrain islet networks.

---

## 6. Organoid and Multi-Physics Models

### 6.1 Pancreatic Islet Organoids: Biology and Computational Interface

Pancreatic islet organoids — either derived from primary islet cells or from stem cell–derived β-cells — have emerged as a major experimental system for studying islet biology in controlled 3D environments. Reviews by **Hohwieler et al. (2017)** and **Balboa et al. (2022)** (*Nat. Biotechnol.* 40:1042–1055) trace the development of stem cell-derived islets (SC-islets). **Wang et al. (*Nature Reviews Endocrinology*, 2023)** surveyed organoid models for diabetes mellitus broadly, assessing their utility for studying pancreatic cell development, physiology, and glucose responses. A very recent preprint (**bioRxiv 2026.04.15**, 2026) reports long-term expansion of human pancreatic islet organoids maintaining α, β, δ, and PP cell ratios, with regulated insulin secretion and glycemic correction in transplantation models.

The computational interface with organoids remains less developed than for primary islets. Most computational models used in organoid contexts are **FEM-based transport models** (oxygen, glucose, insulin diffusion-reaction) rather than biophysically detailed ionic models. The key challenge is scale: organoid-on-chip systems require multiphysics modeling of fluid flow, mass transport, and cell-level secretory function, which is computationally expensive to couple with full ionic β-cell models.

### 6.2 Islet-on-Chip and Microphysiological Systems

**Organoid microphysiological system (MPS)** models have used 2D FEM to predict how continuous perfusion of oxygenated media in a chip platform mitigates hypoxic stress and improves insulin secretion compared to static culture (**Bender, O'Donnell et al., *Sci. Adv.*, 2020**). The model incorporated diffusion-reaction equations for O₂ and a simplified phenomenological insulin secretion function, predicting a biphasic GSIS response and demonstrating that static culture produces ~33% lower AUC of insulin secretion — predictions validated experimentally.

**Quintard et al. (2022)** (*Biomicrofluidics*) designed islet-on-chip devices with computational guidance (first-principles fluid and mass-transport models) for dynamic perifusion GSIS assays. **Parker et al.** and others at the intersection of microfluidics and islet biology have used computational modeling of flow fields and sampling dynamics to guide platform geometry.

### 6.3 Multi-Cell-Type Islet Models

Most biophysical ionic models focus on β-cells. Multi-cell-type models incorporating α and δ cells are less common but growing. **Hoang, Benninger et al. (*PLoS Comput. Biol.*, 2021)** and **Design Principles of Pancreatic Islets (PMC 4818077)** used coupled-oscillator models (Kuramoto-type) for α, β, and δ cells to derive design principles for islet architecture. **Parekh, Thompson, Bertram et al. (*PMC* 2023)** studied paracrine interactions between cell types using mathematical models incorporating α-cell glucagon secretion, δ-cell somatostatin feedback, and β-cell insulin release within the islet micro-environment. **Aizenshtadt et al. (2024)** coupled SC-islets with liver organoids on a recirculating organ-on-chip platform, motivating future computational models of inter-organ crosstalk.

---

## 7. Key Open Problems and Emerging Directions

**1. Coupling ionic models to organoid-scale spatial models.** There is a clear gap between detailed biophysical models of individual β-cells (IOM, Cha-Noma) and the FEM transport models used for organoid-on-chip. Developing multiscale models that embed a few hundred coupled ionic β-cells within a 3D organoid/chip geometry, with realistic O₂ and glucose transport, remains computationally challenging but tractable with modern GPU-accelerated ODE solvers.

**2. Human islet specificity.** The IOM and its predecessors are predominantly parameterized for mouse β-cells. Human islets exhibit different oscillation patterns (predominantly fast), greater α-cell proportion, and different gap-junction properties. Developing and validating a fully human IOM — analogous to the transition from Hodgkin-Huxley (squid axon) to human cardiac ionic models — is an active frontier.

**3. Role of heterogeneity and stochasticity.** Current models indicate that intrinsic single-cell heterogeneity (in glycolytic parameters, ion channel expression) is as important as network topology in shaping collective dynamics. Stochastic channel gating at low copy numbers, especially in small organoid clusters, may qualitatively change dynamics relative to mean-field ODE models. Langevin/stochastic differential equation (SDE) extensions of the IOM are needed.

**4. Paracrine and autocrine signaling.** The roles of glucagon (from α-cells), somatostatin (from δ-cells), and insulin itself in modulating β-cell electrical activity are not fully incorporated in most biophysical models. Mathematical models of paracrine crosstalk (e.g., *PMC* 10461634) remain largely phenomenological.

**5. Hub cell mechanisms.** Despite intensive study, whether hub cells owe their influence primarily to intrinsic electrical excitability, glycolytic capacity, gap-junction connectivity, or paracrine signaling remains unresolved. Coupled IOM models with realistic spatial geometry and cell-type heterogeneity should be able to disambiguate these mechanisms.

**6. Insulin secretion coupling to ionic dynamics.** Most models treat insulin exocytosis as a function of [Ca²⁺]_i with a Hill-type function. More realistic models of the secretory granule pool, docking, priming, and fusion — as modeled in "Pool-and-Trigger" frameworks — could be coupled to the IOM to give quantitative GSIS predictions directly comparable to perifusion assays.

---

## 8. Summary Table of Key Models

| Model | Authors | Year | Framework | Key Feature |
|---|---|---|---|---|
| Chay-Keizer | Chay & Keizer | 1983 | ODE, single cell | Minimal K(Ca) bursting model; fast-slow structure |
| K(ATP) extension | Keizer & Magnus | 1989 | ODE, single cell | K(ATP)-linked bursting; glucose-metabolism link |
| Phantom Burster | Bertram et al. | 1995 | ODE, single cell | Multi-slow-variable bursting; wide timescale range |
| Isopotential supercell | Sherman, Rinzel, Keizer | 1988/1991 | ODE, monodomain | Channel sharing; synchrony from coupling |
| Cha-Noma | Cha et al. | 2011 | ODE, single cell (8 currents) | Detailed mouse β-cell; used in network models |
| Dual Oscillator Model | Bertram, Satin, Sherman | 2004–2007 | ODE, single cell | Ca²⁺ + glycolytic oscillators; compound bursting |
| Ca²⁺ wave spatial model | Benninger et al. | 2008 | ODE network + spatial | Ca²⁺ wave velocity; Cx36 knockdown predictions |
| Integrated Oscillator Model | Marinelli, Fletcher, Bertram, Sherman, Satin | 2021–2023 | ODE, single and network | Symbiotic coupling of Ca²⁺ and metabolism |
| Coupled IOM (β-cell network) | Dwulet, Benninger et al. | 2023 | ODE network | Hub cells; intrinsic dynamics vs. connectivity |
| Multiplex network (human) | Gosak, Marinelli et al. | 2023 | ODE network + graph theory | Human islet; electrical + metabolic coupling layers |
| FEM organoid model | Bender et al. | 2020 | PDE (FEM), spatial | O₂/insulin transport; MPS platform design |

---

## 9. Representative Bibliography

Bertram R, Butte MJ, Kiemel T, Sherman A. Topological and phenomenological classification of bursting oscillations. *Bull. Math. Biol.* 57:413–439, 1995.

Bertram R, Satin LS, Pedersen MG, Luciani DS, Sherman A. Interaction of glycolysis and mitochondrial respiration in metabolic oscillations of pancreatic islets. *Biophys. J.* 92:1544–1555, 2007a.

Bertram R, Sherman A, Satin LS. Metabolic and electrical oscillations: partners in controlling pulsatile insulin secretion. *Am. J. Physiol.* 293:E890–E900, 2007b.

Bertram R, Satin LS, Sherman AS. Closing in on the mechanisms of pulsatile insulin secretion. *Diabetes* 67:351–359, 2018.

Bertram R, Marinelli I, Fletcher PA, Satin LS, Sherman AS. Deconstructing the integrated oscillator model for pancreatic β-cells. *Math. Biosci.* 365:109085, 2023.

Benninger RKP, Zhang M, Head WS, Satin LS, Piston DW. Gap junction coupling and calcium waves in the pancreatic islet. *Biophys. J.* 95:5048–5061, 2008.

Cha CY, Nakamura Y, Himeno Y, et al. Ionic mechanisms and Ca²⁺ dynamics underlying the glucose response of pancreatic β cells: a simulation study. *J. Gen. Physiol.* 138:21–37, 2011.

Chay TR, Keizer J. Minimal model for membrane oscillations in the pancreatic β-cell. *Biophys. J.* 42:181–190, 1983.

Dwulet JM, Briggs JK, Benninger RKP. Small subpopulations of β-cells do not drive islet oscillatory dynamics via gap junction communication. *PLoS Comput. Biol.* 17:e1008948, 2021.

Dwulet JM, Briggs JK, Benninger RKP. β-cell intrinsic dynamics rather than gap junction structure dictates subpopulations in the islet functional network. *eLife* 12:e83147, 2023.

Fazli M, Vo T, Bertram R. Phantom bursting may underlie electrical bursting in single pancreatic β-cells. *J. Theor. Biol.* 501:110346, 2020.

Félix-Martínez GJ, Godínez-Fernández JR. Mathematical models of electrical activity of the pancreatic β-cell: a physiological review. *Islets* 6:e949195, 2014.

Félix-Martínez GJ. A primer on modelling pancreatic islets: from models of coupled β-cells to multicellular islet models. *Islets* 15:2231609, 2023.

Fletcher PA, Marinelli I, Bertram R, Satin LS, Sherman AS. Pulsatile basal insulin secretion is driven by glycolytic oscillations. *Physiology* 37, 2022.

Fletcher PA, Thompson B, Liu C, Bertram R, Satin LS, Sherman AS. Ca²⁺ release or Ca²⁺ entry, that is the question: what governs Ca²⁺ oscillations in pancreatic β cells? *Am. J. Physiol.* 324:E477–E487, 2023.

Gosak M et al. Network science of biological systems at different scales. *Phys. Life Rev.* 24:118–135, 2018.

Johnston NR et al. Beta cell hubs dictate pancreatic islet responses to glucose. *Cell Metab.* 24:389–401, 2016.

Keizer J, Magnus G. ATP-sensitive potassium channel and bursting in the pancreatic beta cell. *Biophys. J.* 56:229–242, 1989.

Lei CL et al. Beta-cell hubs maintain oscillations in human and mouse islet simulations. *Islets* 10:151–167, 2018.

Marinelli I, Fletcher PA, Sherman AS, Satin LS, Bertram R. Symbiosis of electrical and metabolic oscillations in pancreatic β-cells. *Front. Physiol.* 12:781581, 2021.

Marinelli I, Thompson BM, Parekh VS, Fletcher PA, et al. Oscillations in K(ATP) conductance drive slow calcium oscillations in pancreatic β-cells. *Biophys. J.* 121:1449–1464, 2022.

Marinelli I, Parekh V, Fletcher P, Thompson B, et al. Slow oscillations persist in pancreatic beta cells lacking phosphofructokinase M. *Biophys. J.* 121:692–704, 2022.

Marinelli I, Vo T, Gerardo-Giorda L, Bertram R. Transitions between bursting modes in the integrated oscillator model for pancreatic β-cells. *J. Theor. Biol.* 454:310–319, 2018.

McKenna JP, Bertram R. Fast-slow analysis of the integrated oscillator model for pancreatic β-cells. *J. Theor. Biol.* 457:152–162, 2018.

McKenna JP, Dhumpa R, Mukhitov N, Roper MG, Bertram R. Glucose oscillations can activate an endogenous oscillator in pancreatic islets. *PLoS Comput. Biol.* 12:e1005143, 2016.

Peercy BE, Sherman AS. Do oscillations in pancreatic islets require pacemaker cells? *J. Biosci.* 47:14, 2022.

Pré P et al. Both electrical and metabolic coupling shape the collective multimodal activity and functional connectivity patterns in beta cell collectives. *Phys. Rev. E* 108:054409, 2023.

Sherman A, Rinzel J, Keizer J. Emergence of organized bursting in clusters of pancreatic beta-cells by channel sharing. *Biophys. J.* 54:411–425, 1988.

Sherman A, Rinzel J. Model for synchronization of pancreatic beta-cells by gap junction coupling. *Biophys. J.* 59:547–559, 1991.

Smolen P, Keizer J. Slow voltage inactivation of Ca²⁺ currents and bursting mechanisms for the mouse pancreatic beta-cell. *J. Membr. Biol.* 127:9–19, 1992.

Wang X et al. Towards a better understanding of diabetes mellitus using organoid models. *Nat. Rev. Endocrinol.* 19:197–216, 2023.

Watts M, Fendler B, Merrins MJ, Satin LS, Bertram R, Sherman A. Calcium and metabolic oscillations in pancreatic islets: who's driving the bus? *SIAM J. Appl. Dyn. Syst.* 13:683–703, 2014.

---

*Prepared April 2026. Covers literature through early 2026 with emphasis on computational modeling using biophysical ionic β-cell models (Chay-Keizer lineage, DOM, IOM) in monodomain and coupled-oscillator islet/organoid frameworks.*
