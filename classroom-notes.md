# Sage Summer Camp 2026: Field Notes

These notes capture the main ideas, practical lessons, and technical concepts from the Sage Summer Camp. Rather than reproducing each session word for word, they focus on the connections between edge computing, AI, scientific sensing, and autonomous field systems.

---

## Day 1 — Understanding the Sage Ecosystem

### Opening Session with Pete Beckman

#### Camp Deliverables

By the end of the program, participants are expected to:

- Identify a scientific or technical problem to address
- Create a GitHub repository named `sage-summer-camp-2026`
- Maintain a `classroom-notes.md` file
- Prepare a five-minute presentation
- Write a `project.md` overview for the Sage website
- Develop an ECR app, when applicable
- Create a poster
- Build a Hermes “brain”
- Submit a short course review

### The Sage Grande Testbed

Sage is an NSF-funded research testbed designed to explore how advanced computing can support important scientific questions in real-world environments. The project began roughly a decade ago and continues to evolve alongside advances in GPUs, AI, and low-power computing.

The central idea is simple: move intelligence closer to where data is produced. Instead of sending every image, audio recording, or sensor measurement to the cloud, Sage nodes can process data in the field and publish useful results in near real time.

#### Why Edge Computing Matters

| Constraint | Value of edge computing |
| --- | --- |
| Privacy | Sensitive raw data can remain on the device. |
| Bandwidth | Large files can be filtered or summarized before transmission. |
| Power | Local processing may use less energy than continuous data transfer. |
| Latency | Decisions can be made without waiting for a cloud response. |
| Resilience | Individual nodes can continue operating during network or central-system failures. |

Sage supports research ranging from wildlife and pedestrian monitoring to floods, volcanic activity, cloud motion, and animal identification. The broader goal is not simply to deploy sensors, but to create a flexible platform for testing what becomes possible when scientific instruments have local computing and AI capabilities.

The Sage Grande Testbed aims to:

- Develop new AI algorithms and edge technologies
- Improve the safety, privacy, and reliability of AI systems
- Deploy approximately 300 new platforms across the United States
- Enable the next generation of real-time environmental monitoring

Field testing is a major part of the project. Nodes must survive heat, cold, moisture, communication loss, and other conditions that rarely appear in a laboratory benchmark.

### AI Agents at the Edge

AI agents represent a higher level of abstraction in computing. Earlier systems exposed CPUs, operating systems, and cloud services; agents now combine models, tools, memory, and iterative decision-making.

A simplified progression discussed in class was:

- **2022:** Models responded mainly from information learned during training.
- **2023:** Models began using tools and generating code to obtain new information.
- **2025:** Terminal-based agents could operate through autonomous loops.
- **2026:** Agents are becoming persistent, adaptive, model-agnostic, inspectable, and extensible.

Sage is interested in agents for:

- AI-assisted software development
- Orchestration of edge nodes and applications
- Autonomous field operation
- Dynamic interaction with sensors and instruments

Agents can also fail in unpredictable ways. They may follow irrelevant paths, generalize from incomplete knowledge, or lose important context. Good agent design therefore requires constrained tools, careful reasoning, explicit documentation, and recoverable workflows.

### Responsible AI Principles

The session highlighted several practical responsibilities:

- **Transparency:** Clearly identify AI-generated material.
- **Accountability:** The human operator remains responsible for the result.
- **Recovery:** Plan for failures, corruption, and accidental deletion.
- **Privacy:** Understand who can access data, prompts, tokens, and credentials.
- **Reproducibility:** Preserve enough information to repeat the work.
- **Data governance:** Define how data is collected, stored, and reused.
- **Containment:** Limit what an autonomous system can access or change.
- **Key management:** Protect credentials and avoid unnecessary exposure.

### Sage Fundamentals with Sean Shahkarami and Neil Conrad

#### Data Access

Sage provides two main ways to retrieve data:

1. **Sage Data Client**
   - Python-based
   - Queries and organizes results into data frames
   - Recommended for most Python workflows

2. **HTTP Data API**
   - Lower-level access
   - Returns newline-delimited JSON
   - Useful for non-Python applications or custom integrations

Both interfaces provide access to the same underlying data.

- [Accessing Sage data](https://sagecontinuum.org/docs/tutorials/accessing-data)
- [Creating and publishing edge apps](https://sagecontinuum.org/docs/category/edge-apps)

Office hours remain available after the workshop for continued support.

### Day 1 Takeaway

Sage is best understood as a scientific experimentation platform: sensors observe the world, edge computers interpret those observations, and researchers decide which data products are worth publishing.

---

## Day 2 — Connecting Models, Tools, and Learning Systems

### Model Context Protocol with Peter Lebiedzinski

The Model Context Protocol (MCP) is an open standard for connecting AI applications to external systems. It gives a language model a structured way to discover tools, request data, and perform supported actions.

MCP uses JSON-RPC 2.0 and acts as a bridge between natural-language intent and specific services or code. In a Sage workflow, a user might ask a question in plain English, while the MCP layer determines whether it needs to query Sage services, locate data, or invoke another supported operation.

Sage provides its own MCP implementation:

- [Sage MCP repository](https://github.com/sagemcp/sagemcp)

In practical terms, MCP turns natural-language requests into controlled, inspectable tool calls.

### Hermes with Pete Beckman

Hermes supports long-running agent workflows. A key operational lesson was to run persistent processes inside `tmux`, allowing an agent to continue working even after a remote connection closes.

#### Context and Compaction

Agent sessions have finite context. As the session becomes full, the system may compact earlier information into a shorter summary. Important detail can be lost during this process.

A safer workflow is to:

1. Stop before the context is completely full.
2. Write decisions, discoveries, commands, and remaining tasks into a durable file.
3. Begin the next session by loading that file.
4. Continue from documented state rather than relying on imperfect memory.

Token usage also matters. Effective agent workflows avoid repeatedly loading irrelevant content and keep instructions, tool outputs, and notes focused.

### Deep Learning Fundamentals with Chris Lee

#### A Useful Mental Model

Deep learning systems are pattern-recognition systems. They do not produce absolute truth; they estimate probabilities or continuous values from the patterns represented in their training data.

More data can help, but only when the examples are relevant, diverse, correctly labeled, and representative of the environment in which the model will operate.

#### Anatomy of a Neural Network

A neuron combines weighted inputs with a bias and then applies an activation function:

`output = activation(weighted inputs + bias)`

Key terms:

- **Width:** Number of units or features represented across a layer
- **Depth:** Number of hidden and output layers
- **Weights:** Learned influence of each input
- **Bias:** Learned offset
- **Activation:** Nonlinear transformation applied to the neuron output
- **Logits:** Raw outputs produced before classification probabilities

ReLU is a common activation for hidden layers. Output-layer choices depend on the task:

- **Regression:** Predicts a continuous value
- **Classification:** Predicts a class or probability distribution

The Universal Approximation Theorem tells us that neural networks can approximate continuous functions under suitable conditions. In practice, however, simply making a network extremely large does not solve issues of generalization, data quality, or computational cost.

Most importantly, a model cannot reliably infer information that is absent from its inputs and training examples.

#### Dataset Curation

A strong dataset should be:

- Relevant to the task
- Large enough for the intended model
- Diverse
- Accurately labeled
- Representative and reasonably balanced
- Free from train-test contamination

Each supervised example pairs input features with a target label or value. There must be a learnable relationship between them.

A common split is:

- 70% training
- 15% validation
- 15% testing

The test set should be reserved for final evaluation. Repeatedly using test results to make design decisions effectively turns the test set into another validation set.

Stratified splitting helps preserve important class distributions. This is especially important when rare classes would otherwise be missing from one split.

##### Distribution Changes

- **Data drift:** The input distribution changes.
- **Label drift:** The target distribution changes.
- **Concept drift:** The relationship between inputs and targets changes.

Moving a sensor to a substantially different climate, for example, may introduce data drift and could also lead to concept drift.

#### Preprocessing and Augmentation

Typical preprocessing includes:

- Removing duplicates
- Handling missing values
- Correcting inconsistencies
- Reviewing outliers
- Normalizing numeric features
- Encoding categorical variables

Data augmentation may help when training examples are limited, but augmented samples must remain scientifically plausible.

#### Training and Evaluation

Common learning settings include:

- **Supervised learning:** Uses labeled features and targets.
- **Unsupervised learning:** Finds structure without labeled targets.
- **Reinforcement learning:** Learns through interaction, reward, and penalty.

A typical neural-network training loop is:

1. Initialize weights.
2. Run a forward pass.
3. Calculate the loss.
4. Backpropagate gradients.
5. Update weights with an optimizer.

One full pass through the training data is an **epoch**. With mini-batching, the data is shuffled and divided into smaller batches, and model parameters are updated after each batch.

Useful evaluation metrics include:

| Task | Common metrics |
| --- | --- |
| Classification | Accuracy, precision, recall, F1 score |
| Regression | RMSE, MAE, R² |

When training performance continues improving while validation performance degrades, the model is likely overfitting. Early stopping can help prevent this.

Hyperparameters are configuration choices that are not directly learned, such as learning rate, batch size, and model depth. They can be tuned through random search, grid search, Bayesian optimization, or other automated strategies.

### Day 2 Takeaway

Reliable AI depends on two kinds of discipline: structured access to tools and carefully curated access to data. MCP helps control the first; sound machine-learning practice governs the second.

---

## Day 3 — Foundation Models and Scientific Search

### Foundation Models and Edge Inference with Matt Thompson

#### Imageomics

Imageomics uses images to study biological organisms, traits, and observable phenotypes. The initiative is based at Ohio State University and supported by NSF.

Its work addresses a major scientific imbalance: the places receiving the most research attention are not always the places with the greatest biodiversity. Large, diverse, and multimodal datasets can help close that gap.

#### BioCLIP

BioCLIP is a vision-language foundation model designed for biological classification.

- CLIP stands for **Contrastive Language–Image Pretraining**.
- BioCLIP 2.5 was trained on approximately 233 million images covering about one million taxa.
- The model can fit within roughly 4 GB of memory.
- BioCLIP 3 is under development.

Large-scale training can produce emergent representations. The embedding space may capture biological information that was not explicitly labeled and may support new scientific questions.

AI and science have a bidirectional relationship: scientific data improves AI models, while AI can reveal patterns and generate new hypotheses for science.

BioCLIP is intended as a foundation to build on, not a complete solution for every biological task.

##### Options for Domain Adaptation

| Approach | Main idea |
| --- | --- |
| Zero-shot inference | Use the pretrained model directly with no task-specific training. |
| Probe suite | Test whether the desired information already exists in the embedding space. |
| Few-shot probing | Train a lightweight classifier using a small labeled dataset. |
| LoRA | Adapt a limited set of low-rank parameters. |
| Text-tower tuning (LiT) | Tune the language side while preserving the vision encoder. |
| Last-*k* vision blocks | Fine-tune only the final vision layers. |
| Full fine-tuning | Update the entire model at higher computational cost. |
| Continued pretraining | Further train the model on domain-specific data. |

Raw embeddings may sometimes work better than normalized embeddings when training a classification head. A probe suite can reveal whether additional fine-tuning is likely to help before committing substantial compute.

BioCLIP 2.5 has been adapted for plant identification using a phenology mask, although the fine-tuning process was resource-intensive.

##### Making Models Smaller

- **Distillation:** Train a smaller student model using outputs from a larger expert model.
- **Post-training quantization (PTQ):** Reduce numerical precision after training.
- **Quantization-aware training (QAT):** Simulate reduced precision during training.

These methods can reduce memory use and latency, but may also reduce accuracy.

##### Edge Design Constraints

Model design at the edge must account for:

- Power, bandwidth, storage, memory, and compute limits
- Scientific latency and provenance requirements
- Budget and hardware availability
- Difficulty of troubleshooting remote deployments

#### Peromyscus Case Study

*Peromyscus maniculatus* and *Peromyscus leucopus* are visually difficult to distinguish. Their geographic ranges overlap, and one species is a disease vector, making reliable identification scientifically important.

The example demonstrated that a lightweight classifier trained on foundation-model embeddings can outperform a purely zero-shot approach. It is a useful reminder that a smaller task-specific model can become highly effective when it starts from a strong representation.

- [BioCLIP Sage Summer 2026 repository](https://github.com/Imageomics/sage-summer-2026-bioclip)

### Sage Image Search with Francisco Lozano

#### Search Concepts

- **BM25:** Keyword-based ranking
- **Vision-language model (VLM):** Connects visual and textual representations
- **Vector search:** Retrieves semantically similar items by comparing embeddings
- **Vector database:** Stores and indexes embeddings
- **HNSW:** A graph-based index for approximate nearest-neighbor search

Sage Image Search allows researchers to query images from Sage data using natural language. Its benchmarking toolkit supports scalable and reproducible evaluation of retrieval methods.

- [Sage Image Search repository](https://github.com/waggle-sensor/sage-nrp-image-search)

### Day 3 Takeaway

Foundation models are most useful when treated as adaptable representation engines. At the edge, the best solution may combine a strong pretrained encoder with a small, efficient task-specific head.

---

## Day 4 — Making Scientific Data Discoverable and Reusable

### National Data Platform with Pedro Ramonetti and Ismael Perez

Scientific data is difficult to use when access is fragmented, formats are inconsistent, computing requirements are specialized, or collection and sharing workflows are disconnected.

The National Data Platform (NDP) is an ecosystem designed to support collaboration, discovery, and customizable use of data. It helps:

- Catalog datasets so they are more FAIR—findable, accessible, interoperable, and reusable
- Provide collaborative workspaces
- Connect users to national cyberinfrastructure and cloud resources
- Support AI-integrated workflows
- Offer learning tools and reusable examples

#### Catalog and Data Providers

NDP is not primarily a data repository. It catalogs existing data sources and helps users find and connect to them.

Researchers can search through the **Catalog** tab or propose a dataset through **Add to Catalog**. Dataset requests are reviewed by a person. Data providers remain responsible for hosting their data and maintaining accurate metadata.

#### Workspaces and CollabStudio

An NDP workspace can serve as:

- A project environment
- A shared research resource
- A learning environment
- A home for notebooks and curated datasets

Jupyter servers consume resources and should be stopped when no longer needed:

`File → Hub Control Panel → Stop Server`

CollabStudio enables multiple registered NDP users to work in a shared workspace and attach curated catalogs to the project.

### Day 4 Takeaway

Useful scientific infrastructure must do more than store files. It should make data discoverable, supply the computing environment needed to use it, and support collaboration without losing provenance.

---

## Day 5 — Building Reliable Edge Hardware

### Sage Hardware with Raj Sankaran and Yongho Kim

#### Node Types

Sage currently uses two broad node configurations:

| Node | Intended environment | Typical features |
| --- | --- | --- |
| Wild Node | Outdoor field deployment | Rugged enclosure; PoE, USB, and LoRaWAN sensor support |
| Blade Server | Indoor instrument hut or rack | Rugged compute server with convenient PoE sensor expansion |

Waggle is the core software and hardware platform underlying these nodes.

#### What an Edge Device Must Provide

An effective edge system combines:

- **Compute:** A low-power CPU/GPU system-on-chip
- **Instrumentation:** Sensors and, when needed, actuators
- **Communication:** External WAN and internal LAN connections
- **Power:** Reliable AC or DC delivery
- **Management:** Monitoring, debugging, and remote control

Field systems must assume that hardware, software, power, and networks will eventually fail. A robust node should:

- Recover from faults and resume operation
- Restore communication or call home for assistance
- Degrade gracefully when resources are limited
- Preserve a minimum operating state
- Optimize activity to extend its useful lifetime
- Continue operating autonomously during communication blackouts

Systems such as the RCB 600 provide modular power control and safety features to support these requirements.

#### First-Generation Wild Sage Node

The first-generation node is approximately three feet tall, two feet wide, one foot deep, and 30 pounds. It has undergone extensive environmental and electrical testing.

Its two-layer design integrates:

- Computing
- Power
- Environmental conditioning
- Communication
- System-management components

The node exposes four SEN ports for PoE sensors and one SEN port for USB.

### Thor: Next-Generation Compute for Foundation AI

Thor provides 128 GB of unified memory and is being evaluated as a computing platform for larger edge AI models.

Thor systems can perform competitively with DGX Spark on larger models, while DGX Spark may be faster for smaller models. Thor-Blades are intended for indoor rack-style deployments.

Thermal control remains one of the main barriers to placing this level of compute in an outdoor Sage node. As temperature rises, the system reduces power and performance. Cooling approaches under investigation include:

- A copper-tube coil and fan system
- An aluminum enclosure instead of plastic
- Carrier-board and dev-kit configurations with different performance, cooling, and efficiency tradeoffs

The carrier-board option uses more power but maintains more consistent temperatures and higher AI throughput. The dev kit may be more energy-efficient.

#### Supported Sensors and Data Sharing

Sage-compatible instruments include:

- LoRaWAN devices
- Air-quality sensors
- Meteorological instruments
- Pan-tilt-zoom cameras
- Microphones
- Infrared cameras

PyWaggle provides a messaging layer that allows applications to communicate with sensors and share immediate results with other applications.

#### Common Sensor Configurations

- **Networked sensor:** Uses Ethernet/PoE and often exposes HTTP or file-transfer interfaces.
- **USB sensor:** Connects directly and receives power through USB; stability decreases with long cables.
- **Sensor-in-the-box:** Includes a local computer, such as a Raspberry Pi, to manage the sensor and transfer data.
- **Wireless sensor:** Uses a protocol such as LoRaWAN to send small measurements over radio.

### Sensors, Instruments, and Communication Interfaces

#### The Communication Stack

A measurement passes through several layers before it becomes a usable scientific data product:

`Transducer → Electrical interface → Bus/signaling → Transport protocol → Application protocol → Data format → OS/device interface → Application → Data product`

A USB temperature instrument, for example, might follow:

`USB connector → USB bus → CDC-ACM serial → Byte stream → Vendor ASCII protocol → CSV lines → /dev/ttyACM0 → Python app → Temperature data product`

A transducer converts a physical phenomenon into another form, usually an electrical signal.

#### Ways to Connect a Sensor

1. **Direct:** The sensor communicates through a bus exposed by the node.
2. **Networked and wired:** The sensor is an IP endpoint on Ethernet.
3. **Wireless IP:** The device joins a Wi-Fi network.
4. **Microcontroller-mediated:** A microcontroller bridges a raw sensor and the node.
5. **Gateway-mediated:** A gateway aggregates devices, as in LoRaWAN.

As sensors become more distant, lower-powered, or more numerous, systems generally move from direct connections toward networked or gateway-based designs.

#### Sensor, Instrument, and Actuator

- **Sensor:** Converts a physical phenomenon into a signal or measurement.
- **Instrument:** A self-contained measurement system with its own processing.
- **Actuator:** Receives a command and changes the physical world.
- **Software-defined sensor:** Derives a new measurement from other data, such as an ML application that publishes vehicle counts from video.

#### Interface Tradeoffs

| Interface | Strengths | Limitations and best use |
| --- | --- | --- |
| USB | Common, simple, provides data and power | Host-centric, limited power and cable length |
| Ethernet/PoE | Reliable, high bandwidth, long cable runs, one cable for power and data | Requires cabling infrastructure |
| Wi-Fi | Flexible and convenient | Less reliable; best for difficult-to-cable or buffered, low-rate sensors |
| Bluetooth/BLE | Suitable for nearby low-rate battery devices | Short range and sometimes difficult pairing |
| LoRaWAN | Long range and excellent battery life | Very low bandwidth; ideal for dispersed environmental measurements |
| UART | Fundamental serial interface | Baud rates must match |
| RS-232 | Simple legacy point-to-point connection | One device per port and moderate distance |
| RS-485 | Long cables, multiple devices, high robustness | Often requires an adapter and careful bus configuration |
| I²C | Addressed, compact board-level bus | Intended for very short distances |
| SPI | Fast board-level communication | Requires more wires as devices are added |
| GPIO | Direct digital input/output | Low-level and application-specific |
| Analog + ADC | Supports sensors with continuous analog output | Signals are fragile and require conversion and noise control |

LoRa is the physical radio modulation, while LoRaWAN is the networking stack built on top of it:

`Sensor → Gateway → Network server → Application server → Research data`

The range and battery benefits come at the cost of slower and smaller data transfers.

### Day 5 Takeaway

The best edge system is not necessarily the one with the largest model or fastest processor. It is the one that continues producing scientifically useful data under real field constraints.

---

## Day 6 — Agentic Pan-Tilt-Zoom Cameras

### Agentic PTZ Cameras with Peter

An autonomous PTZ camera requires a specialized agent. The proposed PTZ agent is intentionally minimal: it includes only the models and tools needed to operate on a Sage node.

The design has three parts:

#### 1. The Brain — Reasoning

- A local reasoning language model
- Served through Ollama
- Interprets user intent and decides which supported action to take

#### 2. The Eyes — Visual Understanding

- **YOLO:** Detects objects and motion
- **BioCLIP:** Identifies species or taxa
- **Gemma:** Produces semantic scene descriptions

#### 3. The Hands — Sensor Control

- A dedicated gateway is the only component that directly touches the hardware
- The gateway translates approved commands into PTZ movement
- Separating reasoning from hardware control reduces risk and makes the system easier to inspect

The user interacts with the system in plain English, while the agent combines visual evidence, local reasoning, and a constrained hardware interface.

- [Agentic PTZ camera documentation](https://sagecontinuum.org/labs/ptz-app)

### Day 6 Takeaway

An edge agent should not be a general-purpose chatbot with unrestricted hardware access. It should be a small, task-focused system whose perception, reasoning, and control paths are clearly separated.

---

## Final Reflection

Across all six days, one principle appeared repeatedly: successful edge AI is a systems problem.

A useful scientific deployment requires more than an accurate model. It also needs:

- Representative data
- Reliable sensors
- Appropriate communication interfaces
- Efficient local compute
- Recoverable software
- Clear provenance
- Responsible agent behavior
- A well-defined scientific question

Sage brings these pieces together as an experimental platform. Its value lies not only in processing data near the sensor, but in enabling researchers to turn raw observations into timely, reproducible, and scientifically meaningful results.
