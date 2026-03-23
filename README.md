## Hi there 👋

<!--
**AndriiShestov/AndriiShestov** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
flowchart TD
    A[Create InboundOrder\nstatus: DRAFT] --> B[Confirm InboundOrder\nstatus: CONFIRMED]
    B --> C[Create Receipt\nstatus: DRAFT]
    C --> D[Assign Executor\nstatus: ASSIGNED]
    D --> E[Start Receipt\nstatus: IN_PROGRESS]
    E --> F{Receiving mode}
    F -->|No container| G[Receive items to receiving zone]
    F -->|Container flow| H[Create/scan container\nreceive items into container]
    G --> I{Lot / FEFO policy?}
    H --> I
    I -->|Yes| J[Validate lot / expiry\ncreate lot records]
    I -->|No| K[Register stock]
    J --> K
    K --> L{QC required?}
    L -->|Yes| M[Put stock into QC / PENDING_QC\ncreate QC record]
    L -->|No| N[Stock available in receiving area]
    M --> O[QC decision\nRelease / Relabel / Return / Scrap]
    N --> P{Complete receipt?}
    O --> P
    P -->|Partial| Q[InboundOrder = PARTIALLY_RECEIVED]
    P -->|Full| R[Receipt = COMPLETED\nInboundOrder = RECEIVED]
    Q --> S[Create Putaway task / await next receipt]
    R --> S[Create Putaway task]
