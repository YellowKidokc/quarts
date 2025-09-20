#  Faith Through Physics: Series Organization

##  Master Folder Structure

```
Faith Through Physics/                      # Main series folder
├── 00 - Series Index.md                   # Main navigation hub
├── Jesus Series/                          # Jesus metaphors collection
│   ├── 00 - Jesus Series Overview.md      # Introduction to Jesus series
│   ├── OH-1 - Jesus as Light/             # First metaphor (OH = Overview Hub)
│   │   └── JS-O1-Light-Main.md            # Main article only at this level
│   ├── OH-2 - Jesus as Water/             # Second metaphor
│   │   └── JS-O2-Water-Main.md            # Main article only at this level
│   ├── OH-3 - Jesus as Truth/             # Third metaphor
│   │   └── JS-O3-Truth-Main.md            # Main article only at this level
│   └── [Additional OH folders for remaining metaphors]
│
├── God the Father/                        # God metaphors collection
│   ├── 00 - Father Series Overview.md     # Introduction to Father series
│   ├── OH-1 - Father as Quantum Field/    # First metaphor
│   │   └── GF-O1-Field-Main.md            # Main article only at this level
│   └── [Additional metaphor folders]
│
├── Holy Spirit/                           # Spirit metaphors collection
│   ├── 00 - Spirit Series Overview.md     # Introduction to Spirit series
│   ├── OH-1 - Spirit as Entanglement/     # First metaphor
│   │   └── HS-O1-Entanglement-Main.md     # Main article only at this level
│   └── [Additional metaphor folders]
│
├── Quantum Mappings/                      # Cross-reference system
│   ├── 00 - Mapping Overview.md           # Introduction to mappings
│   ├── QM-01 - Wave-Particle Duality.md   # Quantum concept and all spiritual mappings
│   ├── QM-02 - Quantum Entanglement.md    # Quantum concept and all spiritual mappings
│   └── [Additional quantum concept mappings]
│
└── Quantum Faith Explorations/            # Hidden premium content folder
    ├── Jesus Series/                      # Premium content by series
    │   ├── Light/                         # Premium content by metaphor
    │   │   ├── QFE-Light-01-Observer-Effect.md
    │   │   ├── QFE-Light-02-Wave-Collapse.md
    │   │   └── [Additional premium papers]
    │   ├── Water/
    │   │   ├── QFE-Water-01-Quantum-Coherence.md
    │   │   ├── QFE-Water-02-Flow-Physics.md
    │   │   └── [Additional premium papers]
    │   └── [Additional metaphor folders]
    │
    ├── God the Father/
    │   └── [Premium content folders]
    │
    └── Holy Spirit/
        └── [Premium content folders]
```

##  Navigation System

The navigation works as follows:

1. User opens "Faith Through Physics" main folder
2. They see series folders (Jesus Series, God the Father, Holy Spirit, Quantum Mappings)
3. When they open "Jesus Series" they see:
   - Overview document
   - OH-1 through OH-9 folders (for each "I am" statement)
4. When they open any OH folder (e.g., "OH-1 - Jesus as Light"), they only see:
   - The main article (JS-O1-Light-Main.md)

All other content (study materials, assets, etc.) remains hidden in the backend organization but isn't visible at this navigation level.

##  Backend Organization (Hidden from User Navigation)

Behind the scenes, each OH folder still contains the full organization:

```
OH-1 - Jesus as Light/               # What user sees at this level
├── JS-O1-Light-Main.md              # ONLY file visible to user
├── .assets/                         # Hidden folder (note the dot prefix)
│   ├── images/                      # Hidden images
│   └── study_materials/             # Hidden study materials
└── .quantum_faith_explorations/     # Hidden premium links
    └── premium_content_links.md     # Hidden links to premium content
```

##  Premium Content Access

The "Quantum Faith Explorations" premium content is completely separate from the main navigation. Users would access it through:

1. Links in the main articles that point to the premium content
2. A separate premium content portal/page
3. Direct links provided after purchase

##  Implementation Notes

1. **File Naming Convention**:
   - OH-# for Overview Hub folders (visible to users)
   - JS-O# for Jesus Series Overview articles
   - GF-O# for God the Father Overview articles
   - HS-O# for Holy Spirit Overview articles
   - QM-## for Quantum Mapping documents
   - QFE-[Metaphor]-## for Quantum Faith Exploration premium papers

2. **Hiding Support Content**:
   - Use dot prefix (.assets, .research) to hide folders in many systems
   - Alternatively, use a separate backend organization that's linked but not directly navigable

3. **Main Article Format**:
   - Each main article should be completely self-contained
   - Include subtle links to premium "Quantum Faith Explorations" content
   - Focus on clarity and accessibility at this level

This structure keeps the navigation extremely clean while maintaining all the organizational benefits of our previous work. The user only sees the main content path while all the supporting materials and premium content remain organized but hidden from the main navigation.
