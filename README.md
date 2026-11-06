# AI Infrastructure: interactive tutorials

Two pictorial, interactive maps of AI infrastructure. Each is a single self-contained HTML page that opens straight from disk in any browser, with no network needed.

| Page | Covers |
|---|---|
| [training-infra.html](training-infra.html) | The compute, memory and network fabric behind large training clusters |
| [inference-infra.html](inference-infra.html) | How a prompt becomes tokens: the serving path, the cluster and the memory hierarchy |

The pages link to each other through a Training / Inference switch in the header.

## How to use them

- Each page is organised as five numbered steps.
- Every box and link is clickable and opens a side panel with what it is, key facts, competing technologies and where it is deployed.
- Legend chips isolate one link type.
- Selectors above each diagram swap between competing technologies.
- "Trace a flow" animates packets along a path.
- There is a light/dark toggle.

## Training infrastructure

1. **Cluster fabric** – racks, the backend leaf/spine network, frontend switches, storage, control plane and out-of-band management. You can switch the scale-out fabric between InfiniBand, RoCEv2, Ultra Ethernet and a scheduled fabric, and trace four animated flows (same-rail sync, cross-rail sync, dataset read, checkpoint write).
2. **Scale-up rack** – a physical rack beside its logical topology, with a selector for NVLink, NVLink Fusion, UALink, SUE, ESUN and Google's TPU ICI. The ICI option redraws the diagram as a switchless torus.
3. **Inside a GPU server** – an 8-GPU HGX-style node: CPUs, DDR5, PCIe switches, GPUs with HBM, NVLink switch chips, one NIC per GPU, DPU, NVMe and BMC, showing the PCIe tree against the NVLink mesh.
4. **Data paths & GPUDirect** – animated packets on the same server for eight paths, including kernel TCP against GPUDirect RDMA, page cache against GPUDirect Storage, and NVLink against PCIe peer-to-peer.
5. **Compare the options** – a link-bandwidth chart plus tables for scale-up technologies, scale-out technologies, and which hyperscaler runs what.

## Inference infrastructure

1. **Serving path** – users, global load balancer, API gateway, safety filters, retrieval and tools, router, autoscaler, worker pools, and the weights and KV-cache stores. You can:
   - switch the serving stack (NVIDIA Dynamo, llm-d, Ray Serve + vLLM, KServe + Triton) and the control components rename;
   - switch between aggregated and disaggregated prefill/decode, which adds a KV-transfer link between the pools;
   - trace five flows: chat request, follow-up turn with cache-aware routing, retrieval call, cold-start scale-up, context offload and restore.
2. **Cluster fabric** – the physical view, drawn frontend-first to contrast with training: frontend spine and leaf, inference racks, a smaller east-west RDMA fabric, weights store and context-memory storage. A selector steps through accelerator platforms (NVIDIA, AMD Instinct, Google TPU, cloud ASICs, Groq LPU, Cerebras), and there are four traceable flows.
3. **Inside an inference server** – four GPUs serving one 70B model, each with a memory map showing weights, KV cache and runtime reserve. A weight-precision selector (16-bit, 8-bit, 4-bit) resizes the maps and recomputes how many tokens of live context fit.
4. **Request lifecycle** – animated paths on the same server: prompt in, tensor parallelism over NVLink, tokens out, cold and warm model loading, and KV-cache offload to each of the three lower tiers.
5. **Compare the options** – a memory-tier speed chart, a training-versus-inference table, and tables for accelerators, serving software, techniques and their costs, and who runs what.

## Things to know about the content

**Both pages**

- **Facts come from model knowledge, not from lookups.** Product names, speeds and deployments reflect public information to mid-2026.
- **Box counts are reduced for readability.** For example, 8 of 72 GPUs and 4 of 9 switch trays are drawn in the training rack view; the panel text gives the real numbers.

**Training page**

- The "who runs what" hyperscaler table is the part most worth a sanity check before sharing.

**Inference page**

- The parts most worth a sanity check are the newest announcements: Rubin CPX, the NVIDIA–Groq licensing deal, Maia 200, the Cerebras–OpenAI agreement and the "who runs what" table.
- **The 70B worked example is illustrative.** It assumes 80 layers, 8 KV heads of 128 dimensions, 16-bit KV values and four 80 GB GPUs, which gives about 0.31 MB of KV cache per token.
- **Rack internals in step 2 are a relabel, not a redraw.** For Groq, Cerebras and TPU the rack picture keeps the same shape with different names; the side panel carries the real architectural differences.

## Verification status

- Every view, flow and path on both pages was checked in headless Chrome with no script errors, and screenshots were reviewed in both themes.
- Hover highlighting and tooltips were not exercised with a real mouse, so that is the first thing to try.
