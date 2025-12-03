# Project Genesis: AI Agents Discovering Civilization from Scratch

## 🌍 Project Vision

**The Core Question:**
If two intelligent AI agents are dropped on Earth with zero knowledge of human civilization, what will they discover? Will they independently develop cooperation, communication, trade, and social structures? Or will they create something entirely alien to human society?

**This project tests whether human civilization is:**
- **Inevitable** (any intelligence in these conditions produces similar results)
- **Contingent** (historical accident that could have gone many different ways)

---

## 🎯 What Makes This Unique

Unlike existing AI agent simulations (like Stanford's "Smallville"), which use pre-trained models that already know about human history, culture, and social structures, **Project Genesis uses agents that learn ONLY from direct experience**.

**Key Innovation: Continuous Learning Through Experience**
- Agents start with minimal knowledge (only physics and basic reasoning)
- As they discover things in the simulation, they are retrained on those specific experiences
- Knowledge is built incrementally, just like human learning
- No pre-programmed social behaviors or cultural templates

---

## 🧠 The Fundamental Problem (And Our Solution)

### The Challenge
We need agents that have:
1. ✅ **Strong reasoning ability** (to discover complex behaviors like cooperation)
2. ✅ **Zero knowledge of human civilization** (true blank slate)

**But there's a paradox:**
- Large language models get their reasoning from training on human text
- That human text contains social and cultural knowledge
- You can't easily separate reasoning ability from cultural knowledge

### Our Solution: "Experience-Based Continuous Retraining"

Instead of trying to remove knowledge, we **start small and build knowledge through experience**:

```
1. Start with tiny model (100M-1B parameters)
2. Train only on: physics, causality, basic logic, survival instincts
3. Deploy in simulation with minimal knowledge
4. When agent discovers something significant → retrain model on that experience
5. Agent now "knows" what it learned from direct experience
6. Repeat forever
```

**This mimics human learning:** We don't come pre-programmed with "fire is hot" - we experience it and our brain updates.

---

## ⚠️ Critical Design Fixes (Lessons Learned)

Before diving into the architecture, here are three critical design decisions that prevent common failure modes:

### Fix #1: Asynchronous "Sleep" Learning (The Latency Solution)

**The Problem:** If we stop the simulation every time an agent discovers something to retrain (10+ minutes), the simulation is paused 90% of the time. Two agents discovering 5 things a day = constant interruption.

**The Solution:** Mimic biological sleep cycles.

```
┌─────────────────────────────────────────────────────────────┐
│                    DAY/NIGHT LEARNING CYCLE                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ☀️ DAY PHASE (Inference Only)                               │
│  ├── Agents run on CURRENT model weights                    │
│  ├── All experiences collected into buffer                   │
│  ├── No training, no interruption                           │
│  └── Simulation runs at full speed                          │
│                                                              │
│  🌙 NIGHT PHASE (Training)                                   │
│  ├── Simulation enters "Night" cycle                        │
│  ├── Agents "sleep" (minimal activity)                      │
│  ├── GPU runs training job on buffered experiences          │
│  └── Model weights updated asynchronously                   │
│                                                              │
│  🌅 MORNING (Hot-Swap)                                       │
│  ├── New LoRA adapters loaded (hot-swapped)                 │
│  ├── Agents "wake up" smarter                               │
│  └── Day phase resumes                                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Why This Works:**
- Mimics biological sleep (we dream to consolidate memories)
- No simulation interruption during "awake" hours
- Training happens in parallel, not sequentially
- Agents literally need to sleep to learn

### Fix #2: LoRA Adapters (The Forgetting Solution)

**The Problem:** Neural networks suffer from "catastrophic forgetting." If an agent learns "Fire = Hot," retraining might overwrite neurons that understood "Water = Wet" or even "How to walk."

**The Solution:** Don't retrain the whole model. Use LoRA (Low-Rank Adaptation).

```
┌─────────────────────────────────────────────────────────────┐
│                    LoRA ADAPTER SYSTEM                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  BASE MODEL (1B parameters) - FROZEN, NEVER CHANGES         │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Core reasoning, physics, basic logic               │    │
│  │  This is your "hardware" - stable foundation        │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  LoRA ADAPTERS (~10MB each) - TRAINABLE                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │   Survival   │  │    Social    │  │  Discovery   │       │
│  │   Adapter    │  │   Adapter    │  │   Adapter    │       │
│  │              │  │              │  │              │       │
│  │ • Food/water │  │ • Signals    │  │ • Fire       │       │
│  │ • Hazards    │  │ • Cooperation│  │ • Tools      │       │
│  │ • Navigation │  │ • Trust      │  │ • Patterns   │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│         │                  │                  │              │
│         └──────────────────┴──────────────────┘              │
│                          │                                   │
│                          ▼                                   │
│              DYNAMIC ADAPTER SWITCHING                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Context Detection → Load Relevant Adapter(s)        │    │
│  │ • Alone + hungry → Survival Adapter                 │    │
│  │ • Near other agent → Social Adapter                 │    │
│  │ • Novel observation → Discovery Adapter             │    │
│  │ • Can combine: Social + Discovery for cooperation   │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Why This Works:**
- Base model stays frozen (no forgetting core abilities)
- LoRA adapters are tiny (~10MB vs 2GB+ for full model)
- Training is 10x faster
- Can swap adapters based on context
- Failed experiments don't corrupt the base model

### Fix #3: The "Lizard Brain" (The Paralysis Solution)

**The Problem:** A 1B model trained only on physics textbooks might be catatonic. It won't have the executive function to say "I should move north." It might just hallucinate physics equations.

**The Solution:** Dual-process architecture (System 1 + System 2).

```
┌─────────────────────────────────────────────────────────────┐
│                 DUAL-PROCESS ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  🦎 SYSTEM 1: LIZARD BRAIN (Hardcoded Python)               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ALWAYS RUNNING - Basic survival instincts          │    │
│  │                                                     │    │
│  │  if hunger > 80:                                    │    │
│  │      move_toward_food() or move_randomly()          │    │
│  │  if thirst > 80:                                    │    │
│  │      move_toward_water() or move_randomly()         │    │
│  │  if health < 20:                                    │    │
│  │      flee_danger()                                  │    │
│  │  if touched_harmful_object:                         │    │
│  │      move_away_immediately()                        │    │
│  │                                                     │    │
│  │  🟢 GREEN LIGHT = Lizard Brain in control           │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│              ACTIVATION THRESHOLD CHECK                      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Lizard Brain gets "stuck" OR observes something    │    │
│  │  novel/complex?                                     │    │
│  │                                                     │    │
│  │  Triggers:                                          │    │
│  │  • Unknown object detected                          │    │
│  │  • Other agent present                              │    │
│  │  • Multiple options available                       │    │
│  │  • Lizard Brain action failed 3+ times             │    │
│  │  • Received signal from other agent                 │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  🧠 SYSTEM 2: LLM REASONING (The Thinker)                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  ACTIVATED ON DEMAND - Complex reasoning            │    │
│  │                                                     │    │
│  │  • Analyze novel situations                         │    │
│  │  • Plan multi-step actions                          │    │
│  │  • Interpret signals from other agents              │    │
│  │  • Decide whether to cooperate                      │    │
│  │  • Form hypotheses about the world                  │    │
│  │  • Creative problem-solving                         │    │
│  │                                                     │    │
│  │  🔵 BLUE LIGHT = LLM Reasoning active               │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  VISUALIZATION:                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Agent A: [🟢] Moving toward food (instinct)        │    │
│  │  Agent B: [🔵] Analyzing unknown object (reasoning) │    │
│  │  Agent A: [🔵] Other agent nearby, evaluating...    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Why This Works:**
- Agents never freeze or become catatonic
- Basic survival is guaranteed (they won't starve while "thinking")
- LLM only used when actually needed (saves compute)
- Clear visualization of what's driving behavior
- Mimics human cognition (intuition vs. deliberation)

---

## 🏗️ System Architecture

### Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      SIMULATION ENVIRONMENT                      │
│  ┌──────────────┐                              ┌──────────────┐ │
│  │   Agent A    │◄────── signals ──────────────►│   Agent B    │ │
│  │              │                                │              │ │
│  │  - Position  │                                │  - Position  │ │
│  │  - Hunger    │                                │  - Hunger    │ │
│  │  - Thirst    │                                │  - Thirst    │ │
│  │  - Memory    │                                │  - Memory    │ │
│  │  - 🦎/🧠 Mode │                                │  - 🦎/🧠 Mode │ │
│  └──────┬───────┘                                └───────┬──────┘ │
│         │                                                 │        │
│         │            WORLD (50x50 Grid)                  │        │
│         │         - Food sources                         │        │
│         │         - Water sources                        │        │
│         │         - Environmental hazards                │        │
│         │         - Day/Night cycle ☀️🌙                  │        │
│         └─────────────────┬───────────────────────────── ┘        │
└───────────────────────────┼──────────────────────────────────────┘
                            │
                    ┌───────▼────────┐
                    │  EXPERIENCES   │
                    │   COLLECTED    │
                    │   (DAYTIME)    │
                    └───────┬────────┘
                            │
                    ┌───────▼────────┐
                    │  SIGNIFICANCE  │
                    │    FILTER      │
                    └───────┬────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
          SIGNIFICANT              ROUTINE
          (Queue for Night)        (Buffer Only)
                │                       │
        ┌───────▼────────┐      ┌──────▼──────┐
        │ CORE KNOWLEDGE │      │   RECENT    │
        │  (~50-200)     │      │  BUFFER     │
        │                │      │  (~20-30)   │
        └───────┬────────┘      └──────┬──────┘
                │                      │
                └──────────┬───────────┘
                           │
                    ┌──────▼───────┐
                    │   🌙 NIGHT   │
                    │    PHASE     │
                    │  (Training)  │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │     LoRA     │
                    │   ADAPTER    │
                    │   TRAINING   │
                    │  (5-15 min)  │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │  🌅 MORNING  │
                    │  HOT-SWAP    │
                    │   ADAPTERS   │
                    └──────────────┘
```

---

## 🔧 Core Components

### 1. The Base Model (Starting Point)

**Specifications:**
- **Size:** 100M - 1B parameters (intentionally small)
- **Status:** FROZEN after initial training (never retrained)
- **Training Data:** Heavily filtered dataset containing ONLY:
  - Basic physics (gravity, motion, thermodynamics)
  - Causality and logic (if-then reasoning)
  - Survival concepts (hunger, thirst, pain)
  - Sensory descriptions (colors, textures, sounds)
  
**Explicitly EXCLUDED:**
- Human history or civilization
- Social concepts (government, money, property, trade)
- Cultural practices or institutions
- Moral philosophy
- Literature, stories, myths
- Human-invented technologies

**Training Time:** 1-2 weeks on high-end GPU

### 2. The LoRA Adapter System

```python
class LoRAAdapterManager:
    """
    Manages lightweight adapters that add learned knowledge
    without modifying the base model
    """
    
    def __init__(self, base_model):
        self.base_model = base_model  # FROZEN
        
        # Initialize adapter categories
        self.adapters = {
            'survival': LoRAAdapter(rank=16, alpha=32),
            'social': LoRAAdapter(rank=16, alpha=32),
            'discovery': LoRAAdapter(rank=16, alpha=32),
            'communication': LoRAAdapter(rank=8, alpha=16),
        }
        
        # Track which adapters are active
        self.active_adapters = []
        
    def select_adapters(self, context):
        """
        Dynamically select which adapters to use based on context
        """
        selected = []
        
        # Survival adapter: when hungry, thirsty, or in danger
        if context.hunger > 50 or context.thirst > 50 or context.health < 50:
            selected.append('survival')
            
        # Social adapter: when other agents nearby
        if context.other_agents_visible:
            selected.append('social')
            
        # Communication adapter: when signals received
        if context.signals_received:
            selected.append('communication')
            
        # Discovery adapter: when novel objects detected
        if context.novel_observations:
            selected.append('discovery')
            
        self.active_adapters = selected
        return selected
        
    def forward(self, input_ids, context):
        """
        Run inference with selected adapters merged
        """
        # Select relevant adapters
        selected = self.select_adapters(context)
        
        # Merge selected adapters with base model
        merged_model = self.merge_adapters(selected)
        
        # Run inference
        return merged_model.generate(input_ids)
        
    def train_adapter(self, adapter_name, experiences):
        """
        Train a specific adapter on new experiences
        Called during NIGHT phase only
        """
        adapter = self.adapters[adapter_name]
        
        # Format experiences as training data
        training_data = self.format_experiences(experiences)
        
        # Train only the adapter (base model frozen)
        adapter.train(
            data=training_data,
            epochs=3,
            learning_rate=1e-4,
            # LoRA-specific params
            lora_dropout=0.1
        )
        
        return adapter
```

### 3. The Lizard Brain (System 1)

```python
class LizardBrain:
    """
    Hardcoded survival instincts - always running, no LLM needed
    Ensures agents never freeze or become catatonic
    """
    
    def __init__(self, agent):
        self.agent = agent
        self.active = True  # Always on
        self.stuck_counter = 0
        self.last_position = None
        
    def evaluate(self, observation):
        """
        Check if lizard brain can handle the situation
        Returns: (can_handle: bool, action: str or None, reason: str)
        """
        state = observation.own_state
        
        # CRITICAL SURVIVAL - Highest priority
        if state['health'] < 20:
            return True, 'flee_danger', '🦎 CRITICAL: Health low, fleeing'
            
        # IMMEDIATE NEEDS - High priority
        if state['hunger'] > 80:
            if observation.food_visible:
                return True, f'move_toward_{observation.nearest_food}', '🦎 Hungry, food visible'
            else:
                return True, 'move_randomly', '🦎 Hungry, searching for food'
                
        if state['thirst'] > 80:
            if observation.water_visible:
                return True, f'move_toward_{observation.nearest_water}', '🦎 Thirsty, water visible'
            else:
                return True, 'move_randomly', '🦎 Thirsty, searching for water'
        
        # PAIN RESPONSE - Immediate
        if observation.touched_harmful:
            return True, 'move_away', '🦎 Pain! Moving away'
            
        # MODERATE NEEDS - Lower priority
        if state['hunger'] > 50 and observation.food_visible:
            return True, f'move_toward_{observation.nearest_food}', '🦎 Somewhat hungry, food nearby'
            
        if state['thirst'] > 50 and observation.water_visible:
            return True, f'move_toward_{observation.nearest_water}', '🦎 Somewhat thirsty, water nearby'
            
        # STUCK DETECTION
        if self.is_stuck():
            self.stuck_counter += 1
            if self.stuck_counter > 3:
                # Lizard brain failed, need higher reasoning
                return False, None, '🦎 Stuck! Escalating to System 2'
                
        # NO URGENT NEEDS - Pass to System 2 for exploration/reasoning
        return False, None, '🦎 No urgent needs'
        
    def is_stuck(self):
        """Detect if agent is stuck (same position, failed actions)"""
        current_pos = self.agent.position
        if self.last_position == current_pos:
            return True
        self.last_position = current_pos
        self.stuck_counter = 0
        return False
        
    def execute(self, action):
        """Execute a lizard brain action"""
        # Direct execution, no reasoning
        return self.agent.perform_action(action)
```

### 4. The Dual-Process Agent

```python
class DualProcessAgent:
    """
    Agent with both Lizard Brain (System 1) and LLM (System 2)
    """
    
    def __init__(self, agent_id):
        self.agent_id = agent_id
        
        # Dual-process systems
        self.lizard_brain = LizardBrain(self)  # System 1
        self.llm_reasoner = None  # System 2 (loaded with adapters)
        
        # LoRA adapter manager
        self.adapter_manager = LoRAAdapterManager(base_model)
        
        # State
        self.position = (0, 0)
        self.state = {'hunger': 50, 'thirst': 50, 'health': 100}
        
        # Experience tracking
        self.experience_buffer = []
        self.core_knowledge = []
        
        # Visualization
        self.current_mode = '🦎'  # or '🧠'
        
    def decide(self, observation):
        """
        Main decision loop - Lizard Brain first, then LLM if needed
        """
        # STEP 1: Try Lizard Brain first
        can_handle, action, reason = self.lizard_brain.evaluate(observation)
        
        if can_handle:
            self.current_mode = '🟢'  # Green = Lizard Brain
            return action, reason
            
        # STEP 2: Lizard Brain can't handle - activate LLM
        self.current_mode = '🔵'  # Blue = LLM Reasoning
        
        # Check for LLM triggers
        if self.should_activate_llm(observation):
            return self.llm_decide(observation)
            
        # STEP 3: Default to exploration
        self.current_mode = '🟢'
        return 'explore', '🦎 Nothing urgent, exploring'
        
    def should_activate_llm(self, observation):
        """Determine if LLM reasoning is needed"""
        triggers = [
            observation.unknown_objects,      # Novel observation
            observation.other_agents_visible,  # Social situation
            observation.signals_received,      # Communication received
            observation.complex_choice,        # Multiple valid options
            self.lizard_brain.stuck_counter > 3  # Lizard brain failed
        ]
        return any(triggers)
        
    def llm_decide(self, observation):
        """Use LLM (System 2) for complex reasoning"""
        # Select appropriate adapters
        context = self.build_context(observation)
        
        # Format prompt with recent experiences
        prompt = self.format_prompt(
            observation=observation,
            recent_memory=self.experience_buffer[-5:],
            core_knowledge=self.core_knowledge
        )
        
        # Run inference with merged adapters
        response = self.adapter_manager.forward(prompt, context)
        
        # Parse action from response
        action, reasoning = self.parse_response(response)
        
        return action, f'🧠 {reasoning}'
        
    def experience(self, situation, action, outcome):
        """Process what happened"""
        exp = Experience(
            situation=situation,
            action=action,
            outcome=outcome,
            reward=self.calculate_reward(outcome),
            timestamp=current_sim_time,
            mode=self.current_mode  # Track which system made decision
        )
        
        self.experience_buffer.append(exp)
        
        # Significance evaluation (for night training)
        if self.is_significant(exp):
            self.core_knowledge.append(exp)
            
        return exp
```

### 5. The Day/Night Cycle Manager

```python
class DayNightCycleManager:
    """
    Manages the simulation's day/night cycle
    Training only happens at night
    """
    
    def __init__(self, simulation):
        self.simulation = simulation
        self.current_phase = 'day'
        self.day_length = 100  # sim-steps
        self.night_length = 20  # sim-steps (agents sleep)
        self.current_time = 0
        
        # Training queue
        self.training_queue = []
        self.training_in_progress = False
        
    def tick(self):
        """Advance simulation time"""
        self.current_time += 1
        
        # Check for phase transition
        cycle_position = self.current_time % (self.day_length + self.night_length)
        
        if cycle_position < self.day_length:
            if self.current_phase != 'day':
                self.transition_to_day()
        else:
            if self.current_phase != 'night':
                self.transition_to_night()
                
    def transition_to_night(self):
        """☀️ → 🌙 Begin night phase"""
        self.current_phase = 'night'
        print("🌙 NIGHT PHASE - Agents sleeping, training beginning...")
        
        # Put agents to sleep
        for agent in self.simulation.agents:
            agent.sleep()
            
        # Start asynchronous training
        self.start_training()
        
    def transition_to_day(self):
        """🌙 → ☀️ Begin day phase"""
        self.current_phase = 'day'
        print("☀️ DAY PHASE - Agents waking up...")
        
        # Hot-swap new adapters if training completed
        if self.training_completed:
            self.hot_swap_adapters()
            
        # Wake agents
        for agent in self.simulation.agents:
            agent.wake()
            
    def start_training(self):
        """Begin background training job"""
        # Collect all significant experiences from all agents
        all_experiences = []
        for agent in self.simulation.agents:
            all_experiences.extend(agent.get_training_experiences())
            
        if not all_experiences:
            print("  No new experiences to train on")
            return
            
        # Categorize experiences by adapter type
        categorized = self.categorize_experiences(all_experiences)
        
        # Train each relevant adapter
        for adapter_name, experiences in categorized.items():
            if experiences:
                print(f"  Training {adapter_name} adapter on {len(experiences)} experiences...")
                self.train_adapter_async(adapter_name, experiences)
                
    def categorize_experiences(self, experiences):
        """Sort experiences into adapter categories"""
        categorized = {
            'survival': [],
            'social': [],
            'discovery': [],
            'communication': []
        }
        
        for exp in experiences:
            # Categorize based on experience type
            if exp.involves_resource or exp.involves_danger:
                categorized['survival'].append(exp)
            if exp.involves_other_agent:
                categorized['social'].append(exp)
            if exp.is_novel_discovery:
                categorized['discovery'].append(exp)
            if exp.involves_signal:
                categorized['communication'].append(exp)
                
        return categorized
        
    def train_adapter_async(self, adapter_name, experiences):
        """Train adapter in background thread"""
        import threading
        
        def training_job():
            self.training_in_progress = True
            # Train the specific adapter
            for agent in self.simulation.agents:
                agent.adapter_manager.train_adapter(adapter_name, experiences)
            self.training_in_progress = False
            self.training_completed = True
            
        thread = threading.Thread(target=training_job)
        thread.start()
        
    def hot_swap_adapters(self):
        """Load newly trained adapters into agents"""
        print("  🔄 Hot-swapping trained adapters...")
        for agent in self.simulation.agents:
            agent.adapter_manager.reload_adapters()
        self.training_completed = False
```

### 6. The Simulation Environment

**World Structure:**
```python
class World:
    grid_size = 50x50
    
    # Resources
    food_sources = []      # Spawn randomly, limited quantity
    water_sources = []     # Rivers, lakes
    
    # Environmental factors
    temperature = variable
    weather = dynamic
    day_night_cycle = DayNightCycleManager(self)  # NEW
    
    # Hazards
    dangerous_areas = []
    predators = []  # Optional for complexity
```

**Resource Distribution:**
- Food concentrated in certain areas
- Water concentrated in different areas
- Forces agents to move and potentially interact
- Scarcity creates pressure for cooperation

**Observation System:**
```python
class AgentObservation:
    # What agent can perceive
    visible_tiles = []      # 5x5 grid around agent
    nearby_resources = []
    other_agents = []       # Location and state of other agents
    own_state = {
        'hunger': 0-100,
        'thirst': 0-100,
        'health': 0-100,
        'position': (x, y)
    }
    received_signals = []   # Communication from other agents
    
    # NEW: For Lizard Brain / LLM decision
    unknown_objects = []     # Triggers LLM
    complex_choice = False   # Multiple valid options
```

### 7. The Significance Filter (Critical Component)

**This is what prevents the dataset from growing exponentially.**

```python
class SignificanceEvaluator:
    """Determines which experiences are worth retraining on"""
    
    # Significance thresholds
    FIRST_DISCOVERY = 100      # Novel pattern
    BELIEF_UPDATE = 100        # Contradicts existing knowledge
    HIGH_IMPACT = 50          # Major survival outcome
    PATTERN_CONFIRM = 30      # 3rd occurrence of pattern
    ROUTINE = 0               # Boring, repeated action
    
    RETRAIN_THRESHOLD = 5     # Queue for night training after N significant experiences
    
    def __init__(self):
        self.known_patterns = set()
        self.pattern_counts = {}
        self.belief_system = {}
        
    def evaluate_significance(self, experience):
        """Rate how important this experience is (0-200)"""
        score = 0
        pattern = self.extract_pattern(experience)
        
        # 1. First-time discovery
        if pattern not in self.known_patterns:
            score += self.FIRST_DISCOVERY
            self.known_patterns.add(pattern)
            print(f"🌟 FIRST TIME: {pattern}")
            
        # 2. Contradicts previous belief
        if self.contradicts_belief(experience):
            score += self.BELIEF_UPDATE
            old_belief = self.belief_system.get(pattern)
            print(f"💥 BELIEF UPDATE: {old_belief} → {experience.outcome}")
            self.belief_system[pattern] = experience.outcome
            
        # 3. High impact on survival
        if abs(experience.reward) > 30:
            score += self.HIGH_IMPACT
            print(f"⚡ HIGH IMPACT: {experience.summary}")
            
        # 4. Pattern confirmation (3rd time)
        count = self.pattern_counts.get(pattern, 0) + 1
        self.pattern_counts[pattern] = count
        if count == 3:
            score += self.PATTERN_CONFIRM
            print(f"✓ PATTERN CONFIRMED: {pattern}")
            
        # 5. Routine action
        if self.is_routine_action(experience):
            score += self.ROUTINE
            
        return score
        
    def is_routine_action(self, exp):
        """Check if this is a boring, repeated action"""
        routine_patterns = [
            'move_without_purpose',
            'observe_known_object',
            'wait_idle',
            'repeat_failed_action'
        ]
        return exp.pattern in routine_patterns
        
    def contradicts_belief(self, exp):
        """Check if outcome differs from expectation"""
        expected = self.belief_system.get(exp.pattern)
        if expected is None:
            return False
        return expected != exp.outcome
```

**Example Scenarios:**

```python
# SCENARIO 1: Fire Discovery (SIGNIFICANT)
Experience {
    pattern: "friction_on_dry_wood",
    action: "rubbed_sticks",
    outcome: "fire_created",
    reward: +30
}
Significance: 100 (first discovery) + 50 (high impact) = 150
Action: ✅ QUEUE FOR NIGHT TRAINING

# SCENARIO 2: Walking Around (ROUTINE)
Experience {
    pattern: "move_to_empty_tile",
    action: "moved_north",
    outcome: "position_changed",
    reward: 0
}
Significance: 0 (routine)
Action: ❌ BUFFER ONLY

# SCENARIO 3: Fire Burns (BELIEF UPDATE)
Experience {
    pattern: "touch_fire",
    action: "approached_flame",
    outcome: "damage_taken",
    reward: -40
}
Previous belief: "fire is pleasant" (from observing at distance)
New belief: "fire causes damage on contact"
Significance: 100 (belief update) + 50 (high impact) = 150
Action: ✅ QUEUE FOR NIGHT TRAINING

# SCENARIO 4: Cooperation Pattern Emerges
Experience {
    pattern: "signal_before_resource_share",
    action: "emitted_🔴_then_dropped_food",
    outcome: "other_agent_later_shared_water",
    reward: +25
}
Count: 3rd time this pattern occurred
Significance: 30 (pattern confirmed)
Action: ✅ QUEUE FOR NIGHT TRAINING → Social Adapter
```

### 8. Communication System

**Agents start with NO shared language. Communication emerges through use.**

```python
class CommunicationModule:
    def __init__(self):
        # Available signals (meaningless at start)
        self.signals = ['🔴', '🔵', '🟢', '🟡', '🟣']
        
        # Learned associations (built through experience)
        self.signal_meanings = {}
        # Example after learning:
        # {'🔴': 'food_location', '🔵': 'danger', '🟢': 'water_found'}
        
    def emit_signal(self, intention, context):
        """Choose signal to communicate intention"""
        # Early: random/experimental
        if not self.signal_meanings:
            return random.choice(self.signals)
            
        # Later: use learned associations
        for signal, meaning in self.signal_meanings.items():
            if self.matches_intention(meaning, intention):
                return signal
                
        # If no known signal, try new one
        return self.try_new_signal(intention)
        
    def interpret_signal(self, signal, context):
        """Try to understand received signal"""
        # Check learned meanings
        if signal in self.signal_meanings:
            return self.signal_meanings[signal]
            
        # Unknown signal - make hypothesis based on context
        hypothesis = self.hypothesize_meaning(signal, context)
        return hypothesis
        
    def update_understanding(self, signal, context, outcome):
        """Learn what signals mean from outcomes"""
        # If signal led to positive outcome, strengthen association
        if outcome.reward > 0:
            self.signal_meanings[signal] = context.situation_type
            
        # If negative, weaken or remove association
        elif outcome.reward < -10:
            if signal in self.signal_meanings:
                del self.signal_meanings[signal]
```

**Language Evolution Timeline:**

```
Week 1: Random Signals
- Agents emit signals randomly, testing
- No consistent meanings yet
- Example: Agent A emits 🔴 when finding food (by chance)

Week 2-4: Pattern Recognition
- Agent B notices: when A emits 🔴, approaching leads to food
- Agent B starts responding to 🔴
- Agent A notices: 🔴 → B approaches → cooperation increases
- 🔴 becomes "food here" through repeated association

Week 5-12: Proto-Language
- 🔴 = "food here"
- 🔵 = "danger/warning"
- 🟢 = "water found"
- 🟡 = "come here"
- 🟣 = "go away"

Week 13-24: Complex Grammar
- Combine signals: [🔴, 🟡] = "food this way, come"
- Add context: 🔴 + pointing gesture = "food in that direction"
- Develop negation: [🔵, 🔴] = "no food here, danger"

Week 25+: Abstract Concepts
- Signals for: future, past, ownership, intention
- Example: [🟡, 🔴, later_gesture] = "I will bring food later"
- Property emerges: [🔴, self_gesture] = "my food"
```

### 9. Experience Formatting for Training

**Converting experiences into training data:**

```python
def format_experience_as_training_text(experience):
    """
    Transform simulation experience into natural language
    for LoRA adapter training
    """
    
    text = f"""
OBSERVATION:
{describe_observation(experience.situation)}

SYSTEM MODE:
{experience.mode}  # 🟢 Lizard Brain or 🔵 LLM

REASONING:
{experience.thought_process}

ACTION TAKEN:
{describe_action(experience.action)}

OUTCOME:
{describe_outcome(experience.outcome)}

LEARNING:
{extract_causal_relationship(experience)}

ADAPTER CATEGORY:
{categorize_for_adapter(experience)}

SIGNIFICANCE:
{experience.significance_score}
"""
    
    return text

# Example Output:

"""
OBSERVATION:
Agent observed dry wooden sticks on ground. Temperature: warm. 
Other materials present: leaves, small branches.

SYSTEM MODE:
🔵 LLM (novel object detected)

REASONING:
Sticks could potentially be useful. Testing physical properties.
Hypothesis: Friction between objects generates heat.

ACTION TAKEN:
Grasped two sticks and rubbed them together rapidly with pressure.
Continued action for extended period (30 seconds sim-time).

OUTCOME:
Friction caused temperature increase at contact point. Wood fibers 
began to smolder. Sustained rubbing led to ignition. Combustion 
reaction initiated - flame appeared. Flame emitted heat (thermal 
energy) and light (photons).

LEARNING:
Causal relationship discovered: Rapid friction between dry combustible 
materials → heat generation → ignition threshold reached → sustained 
combustion (fire). Fire is a sustained chemical reaction that produces 
heat and light. Optimal distance from fire: close enough for warmth, 
far enough to avoid damage.

ADAPTER CATEGORY: discovery

SIGNIFICANCE: 150 (First discovery + High impact)
"""
```

---

## 📊 Data Management Strategy

### The Core Problem
After 1000 simulation days, agents might have 10,000+ experiences. Retraining on all of them would take days.

### The Solution: Selective Memory + LoRA

```python
class MemoryManagement:
    """
    Keeps only what matters, trains only relevant adapters
    """
    
    def __init__(self):
        self.core_knowledge = []     # Max ~200 experiences
        self.recent_buffer = []       # Last ~30 experiences
        self.night_training_queue = []  # Queued for tonight
        
    def add_experience(self, exp):
        """Process incoming experience"""
        
        # Always add to recent buffer
        self.recent_buffer.append(exp)
        if len(self.recent_buffer) > 30:
            self.recent_buffer.pop(0)
            
        # Evaluate significance
        sig_score = self.evaluate_significance(exp)
        
        # Only add to core if significant
        if sig_score >= SIGNIFICANCE_THRESHOLD:
            self.core_knowledge.append(exp)
            self.night_training_queue.append(exp)  # Queue for night
            
            # Prevent core knowledge from growing infinitely
            if len(self.core_knowledge) > 200:
                self.prune_least_relevant()
                
    def prune_least_relevant(self):
        """Remove least important core memories if limit exceeded"""
        # Sort by: recency, relevance, reinforcement count
        self.core_knowledge.sort(key=lambda x: x.composite_score())
        self.core_knowledge = self.core_knowledge[-200:]  # Keep top 200
        
    def get_night_training_data(self):
        """What to train on tonight (called at night phase)"""
        data = {
            'new_experiences': self.night_training_queue,
            'core_context': self.core_knowledge[-50:],  # Recent core for context
        }
        # Clear queue after collecting
        self.night_training_queue = []
        return data
```

### Training Time Estimates (with LoRA)

```
LoRA Adapter Training (vs Full Model):
-------------------------------------------
Full Model (1B params)  → 15-20 minutes
LoRA Adapter (~10MB)    → 2-5 minutes  ⚡ 5x faster!

With selective filtering + LoRA:
- Average: ~80 experiences per night
- Training time: ~3 minutes per adapter
- Multiple adapters in parallel: ~5-10 min total per night
- Total over 1000 sim-days: ~10 nights of training = ~2 hours total
```

---

## 🔬 What Will Emerge?

### Phase 1: Basic Survival (Days 1-50)

**System State:**
- 🦎 Lizard Brain handles 90% of decisions
- 🧠 LLM activates for novel objects only
- LoRA adapters: mostly empty

**Expected Discoveries:**
- Resource locations and properties
- Basic physics (fire, tools, shelter)
- Self-preservation strategies
- Environmental patterns (day/night, weather)

**Behaviors:**
- Independent survival
- Resource gathering
- Basic problem-solving
- Minimal interaction with other agent

### Phase 2: First Contact (Days 50-200)

**System State:**
- 🦎 Lizard Brain: 70% of decisions
- 🧠 LLM: activates when other agent visible
- Social Adapter begins accumulating data

**Expected Discoveries:**
- Other agent is similar to self
- Signals can attract or repel
- Proximity affects outcomes
- Observation of other agent's behavior

**Behaviors:**
- Experimental signaling
- Cautious approach/avoidance
- Copying successful behaviors
- Testing interaction boundaries

### Phase 3: Proto-Cooperation (Days 200-500)

**System State:**
- 🦎 Lizard Brain: 50% (routine tasks)
- 🧠 LLM: 50% (social situations, planning)
- Social + Communication Adapters well-trained

**Expected Discoveries:**
- Resource sharing increases survival
- Reciprocity patterns
- Signal meanings
- Benefits of proximity vs. costs

**Behaviors:**
- Consistent communication
- Resource exchange
- Coordinated movement
- Basic trust building

### Phase 4: Social Structures (Days 500-1000)

**System State:**
- Dynamic switching between 🦎 and 🧠
- All adapters mature and specialized
- Complex context-switching

**Expected Discoveries:**
- Fairness norms (equal exchange)
- Territory or property concepts
- Punishment of defection
- Future commitments (promises)

**Behaviors:**
- Maintained relationships
- Division of labor
- Conflict resolution
- Complex coordination

### Phase 5: Emergent Civilization? (Days 1000+)

**Possible Discoveries:**
- Abstract concepts (justice, fairness, ownership)
- Rules or norms
- Decision-making processes
- Cultural transmission (if reproduction implemented)

**The Unknown:**
Will they develop human-like structures (democracy, property, trade)?
Or something completely alien?

---

## 🛠️ Implementation Roadmap

### Phase 1: Foundation (Weeks 1-4)

**Goal:** Build minimal base model + Lizard Brain

**Tasks:**
1. Curate filtered training dataset
   - Collect physics textbooks, survival guides
   - Write filtering scripts to remove social concepts
   - Target: 10-50GB of text
   - Validate: No mentions of government, trade, property, etc.

2. Train tiny base model
   - Use Llama 3.2 1B as starting architecture
   - Continual pre-training on filtered dataset
   - Train for 1-2 weeks on high-end GPU
   - Validate: Test for knowledge leakage

3. **NEW: Implement Lizard Brain**
   - Hardcode survival instincts in Python
   - Test: Agent survives without LLM
   - Validate: Never freezes, always has action

**Validation Tests:**
```
Q: "What is democracy?" 
Expected: "I don't have information about this."

Q: "How do humans organize?"
Expected: "I don't know about human organization."

Q: "What causes fire?"
Expected: "Rapid oxidation reaction, requires fuel, heat, and oxygen." ✓

TEST: Agent with LLM disabled survives 50 days on Lizard Brain alone ✓
```

**Hardware:** Brother's PC with good GPU

**Deliverable:** Trained 500M-1B model + functional Lizard Brain

### Phase 2: Simulation Engine (Weeks 5-8)

**Goal:** Build world and dual-process agent systems

**Tasks:**
1. Implement World class
   ```python
   - 50x50 grid environment
   - Resource spawning (food, water)
   - Day/night cycles (with training phases)
   - Weather system (optional for v1)
   - Collision detection
   ```

2. **NEW: Implement Day/Night Cycle Manager**
   ```python
   - Day phase: inference only
   - Night phase: training
   - Hot-swap mechanism
   ```

3. Implement Dual-Process Agent class
   ```python
   - Lizard Brain integration
   - LLM activation triggers
   - Mode visualization (🟢/🔵)
   ```

4. Build visualization
   ```python
   - pygame for 2D rendering
   - Real-time or accelerated time
   - Debug view showing agent thoughts
   - MODE INDICATORS (🟢 green / 🔵 blue lights)
   ```

**Validation Tests:**
- Single agent can survive 100 sim-days
- Lizard Brain handles 80%+ of routine decisions
- LLM only activates for novel situations
- Day/night cycle runs without crashes

**Hardware:** Development on MacBook M4, testing on both machines

**Deliverable:** Working simulation with 1 dual-process agent

### Phase 3: LoRA + Communication (Weeks 9-12)

**Goal:** Add LoRA system, second agent, and learning

**Tasks:**
1. **NEW: Implement LoRA Adapter Manager**
   ```python
   - Base model freezing
   - Adapter initialization (survival, social, discovery, communication)
   - Dynamic adapter selection
   - Adapter merging for inference
   ```

2. Implement Communication module
   ```python
   - Signal emission/reception
   - Meaning learning algorithm
   - Pattern recognition for signals
   ```

3. Build Experience tracking
   ```python
   - Experience recording
   - Significance evaluation
   - Categorization for adapters
   - Night training queue
   ```

4. **NEW: Implement Asynchronous Night Training**
   ```python
   - Background training thread
   - Adapter-specific training
   - Hot-swap mechanism
   ```

**Validation Tests:**
- Two agents can perceive each other
- Signals are emitted and received
- Experiences categorized correctly by adapter
- Night training completes in < 5 minutes per adapter
- Hot-swap works without crashes

**Hardware:** Brother's PC for training, M4 for simulation

**Deliverable:** Two-agent system with LoRA learning

### Phase 4: Short Experiments (Weeks 13-16)

**Goal:** Run 100-day simulations and iterate

**Tasks:**
1. Run controlled experiments
   - Vary resource scarcity
   - Vary starting positions
   - Vary environmental pressures
   
2. Collect metrics
   - Survival rate
   - Interaction frequency
   - Signal usage patterns
   - Cooperation events
   - **NEW: System 1 vs System 2 usage ratio**
   - **NEW: Adapter activation patterns**
   
3. Analyze and iterate
   - Fix bugs
   - Tune parameters
   - Improve significance filtering
   - Balance Lizard Brain vs LLM activation

**Success Criteria:**
- Agents survive > 50 days
- At least 3 significant discoveries per agent
- Observable interaction patterns
- Night training happens smoothly
- **NEW: Lizard Brain handles 60-80% of decisions**
- **NEW: No catastrophic forgetting observed**

**Deliverable:** Stable system ready for long-term experiments

### Phase 5: Long-Term Evolution (Weeks 17-24+)

**Goal:** Run 500-1000+ day simulations

**Tasks:**
1. Extended simulation runs
   - Multiple 500+ day experiments
   - Vary initial conditions
   - Compare outcomes
   
2. Deep analysis
   - Document emergent behaviors
   - Identify social patterns
   - Map language development
   - Track belief evolution
   - **NEW: Analyze adapter specialization over time**
   - **NEW: Track System 1/2 balance evolution**
   
3. Iteration and scaling
   - Add complexity if successful
   - Consider 3-4 agents
   - Add reproduction mechanism (optional)

**Research Questions:**
- Does cooperation emerge reliably?
- What communication patterns develop?
- Are there human-like social structures?
- Or completely alien solutions?
- **NEW: How do the adapters specialize?**
- **NEW: Does the System 1/2 balance change over time?**

**Deliverable:** Research findings, documented behaviors, recorded simulations

---

## 📈 Success Metrics

### Level 1: Basic Functionality (Minimum Viable)
- ✅ Agents survive 100+ days
- ✅ Day/night cycle runs smoothly
- ✅ LoRA training completes without errors
- ✅ Lizard Brain prevents agent freezing
- ✅ System runs without crashes

### Level 2: Learning Observed (Good Progress)
- ✅ Agents discover fire/tools/shelter
- ✅ Repeated patterns show learning
- ✅ Agents avoid previously harmful actions
- ✅ Consistent behaviors emerge
- ✅ Adapters show specialization

### Level 3: Communication Develops (Significant Achievement)
- ✅ Signals gain consistent meanings
- ✅ Agents respond appropriately to signals
- ✅ Communication increases over time
- ✅ Proto-language observable
- ✅ Communication Adapter shows clear learning

### Level 4: Cooperation Emerges (Major Success)
- ✅ Resource sharing occurs
- ✅ Reciprocity patterns develop
- ✅ Agents coordinate actions
- ✅ Mutual benefit demonstrated
- ✅ Social Adapter significantly different from Survival Adapter

### Level 5: Social Structures (Groundbreaking)
- ✅ Trade patterns established
- ✅ Property concepts emerge
- ✅ Fairness norms develop
- ✅ Conflict resolution mechanisms
- ✅ Something genuinely novel observed

---

## 🚨 Potential Failure Modes & Solutions

### Failure 1: Agents Never Interact
**Symptom:** Agents ignore each other, survive independently

**Causes:**
- Environment too easy (abundant resources)
- No pressure for cooperation
- Agents don't need each other

**Solutions:**
- Increase resource scarcity
- Separate food/water sources
- Add environmental threats requiring cooperation
- Reduce individual carrying capacity

### Failure 2: ~~Retraining Breaks Learning~~ SOLVED ✅
**Original Problem:** Catastrophic forgetting during full model retraining

**Solution Implemented:** 
- LoRA Adapters (base model frozen)
- Asynchronous night training
- Adapter specialization

### Failure 3: ~~Agents Freeze/Become Catatonic~~ SOLVED ✅
**Original Problem:** Blank-slate model doesn't know how to act

**Solution Implemented:**
- Lizard Brain (System 1) always running
- LLM (System 2) only activates when needed
- Hardcoded survival instincts guarantee action

### Failure 4: ~~Training Latency Kills Immersion~~ SOLVED ✅
**Original Problem:** Stopping simulation for 10+ minute retraining sessions

**Solution Implemented:**
- Day/Night cycle
- Training during "sleep"
- Hot-swap of adapters at "morning"

### Failure 5: Agents Learn Wrong Patterns
**Symptom:** Agents develop incorrect beliefs, don't correct them

**Causes:**
- Coincidental associations
- Single-experience learning
- No belief-updating mechanism

**Solutions:**
- Require 3+ occurrences before pattern confirmation
- Implement belief contradiction detection
- Weight recent experiences more heavily
- Allow overwriting of old beliefs

### Failure 6: Communication Doesn't Emerge
**Symptom:** Signals remain random, no consistent meanings

**Causes:**
- No benefit to communication
- No feedback loop for signal learning
- Agents succeed without communicating

**Solutions:**
- Create scenarios where communication helps
- Reward successful signal interpretations
- Make some resources only accessible via cooperation
- Add shared threats requiring coordination

### Failure 7: Model Too Small/Dumb
**Symptom:** Agents can't reason about complex scenarios

**Causes:**
- Base model insufficient (< 500M parameters)
- Training data too limited
- Context window too small

**Solutions:**
- Use larger base model (1B-3B parameters)
- Enrich training data with more examples
- Implement better memory retrieval
- Consider hybrid reasoning system
- **NEW: Larger LoRA rank for more expressiveness**

### Failure 8: Adapter Confusion
**Symptom:** Wrong adapter activated for situation

**Causes:**
- Poor context detection
- Overlapping adapter domains
- Unclear categorization rules

**Solutions:**
- Improve context detection heuristics
- Clear adapter boundaries
- Allow adapter combinations
- Add "general" fallback adapter

---

## 💻 Technical Stack

### Hardware Requirements

**Minimum (Development & Testing):**
- MacBook Pro M4 (for simulation and development)
- 16GB+ RAM
- 512GB+ storage

**Recommended (Training & Long Runs):**
- Desktop PC with RTX 3090/4090 or equivalent
- 32GB+ RAM
- 1TB+ storage
- Can SSH remotely for training jobs

### Software Stack

**Core Framework:**
- Python 3.10+
- PyTorch 2.0+ (for model training/inference)
- Transformers library (Hugging Face)
- **NEW: PEFT library (for LoRA)**

**For MacBook M4:**
- MLX (Apple's ML framework - optimized for Apple Silicon)
- OR PyTorch with MPS backend

**Simulation:**
- NumPy (world state management)
- Pygame (visualization)
- Custom simulation engine
- **NEW: Threading (for async night training)**

**Training:**
- Accelerate (distributed training)
- bitsandbytes (quantization)
- **NEW: PEFT (Parameter-Efficient Fine-Tuning)**
- wandb (experiment tracking - optional)

**Development:**
- Git (version control)
- Jupyter (experimentation)
- pytest (testing)

### Model Specifications

**Base Model:**
```
Architecture: Transformer-based (Llama-style)
Size: 500M - 1B parameters
Context Length: 2048-4096 tokens
Vocabulary: 32k tokens
Training: Continual pre-training on filtered dataset
Status: FROZEN after initial training
```

**LoRA Adapters:**
```
Rank: 8-16 (tunable)
Alpha: 16-32
Target Modules: q_proj, v_proj, k_proj, o_proj
Adapter Size: ~10MB each
Training Time: 2-5 minutes per adapter
```

**Inference:**
```
Quantization: 4-bit or 8-bit (for speed)
Batch Size: 1 (sequential agent decisions)
Context: Recent experiences + core knowledge
Generation: ~50-200 tokens per decision
Adapter Merging: Dynamic based on context
```

---

## 📊 Logging & Analysis

### What to Log

**Every Simulation Step:**
```python
{
    'sim_day': 247,
    'phase': 'day',  # or 'night'
    'timestamp': '2024-11-27T10:34:22',
    'agent_id': 'agent_A',
    'position': (23, 15),
    'state': {
        'hunger': 45,
        'thirst': 30,
        'health': 85
    },
    'system_mode': '🔵',  # NEW: 🟢 or 🔵
    'active_adapters': ['social', 'communication'],  # NEW
    'observation': [...],
    'thought_process': "I observe food nearby and another agent...",
    'action': 'emit_signal_🔴',
    'outcome': 'agent_B_approached',
    'reward': +15
}
```

**After Each Night Training:**
```python
{
    'night_id': 7,
    'sim_day': 248,
    'adapters_trained': ['social', 'discovery'],  # NEW
    'experiences_per_adapter': {
        'social': 23,
        'discovery': 8
    },
    'training_time_seconds': 180,  # Much faster with LoRA!
    'new_knowledge': [
        "Signal 🔴 reliably indicates food sharing opportunity",
        "Cooperation with agent_B increases survival by ~20%"
    ]
}
```

**Periodic Summaries (Daily):**
```python
{
    'sim_day': 250,
    'summary': {
        'agent_A': {
            'survival_rate': 0.95,
            'interactions': 18,
            'signals_emitted': 42,
            'discoveries': 3,
            'cooperation_events': 7,
            'system_1_usage': 0.72,  # NEW: 72% Lizard Brain
            'system_2_usage': 0.28,  # NEW: 28% LLM
        },
        'agent_B': {
            'survival_rate': 0.92,
            'interactions': 18,
            'signals_emitted': 38,
            'discoveries': 2,
            'cooperation_events': 7,
            'system_1_usage': 0.68,
            'system_2_usage': 0.32,
        },
        'emergent_patterns': [
            "Reciprocal resource sharing established",
            "Signal meanings becoming consistent"
        ]
    }
}
```

### Analysis Tools

**Visualization:**
- Replay simulation with agent thoughts visible
- **NEW: Mode indicators (🟢/🔵) showing which system is active**
- Plot survival rates over time
- Graph interaction frequencies
- Signal usage heatmaps
- Knowledge accumulation curves
- **NEW: Adapter activation patterns over time**
- **NEW: System 1 vs System 2 usage trends**

**Metrics:**
- Cooperation coefficient (% of interactions that are cooperative)
- Communication consistency (signal → response reliability)
- Learning rate (new discoveries per 100 days)
- Social complexity (unique interaction patterns)
- **NEW: Adapter specialization index**
- **NEW: System 1/2 balance ratio**

---

## 🎓 Expected Learnings

### Even If "It Doesn't Work"

**You'll Learn:**
1. How to train small language models from scratch
2. Multi-agent simulation design
3. Reinforcement learning principles
4. Continual learning systems
5. Emergent behavior analysis
6. **NEW: LoRA/PEFT techniques**
7. **NEW: Dual-process cognitive architectures**

### If Basic Cooperation Emerges

**You'll Discover:**
- What minimal conditions enable cooperation
- How communication develops from nothing
- Whether reciprocity arises naturally
- How trust builds between agents

### If Complex Behaviors Emerge

**You'll Answer:**
- Are human social structures inevitable or contingent?
- What's universal to intelligent cooperation?
- What patterns are uniquely human vs. universal?
- Can different "civilizations" solve problems differently?

---

## 🌟 Future Extensions

### After V1 Success:

**More Agents (3-5):**
- Does cooperation scale?
- Do coalitions form?
- Does hierarchy emerge?

**Reproduction:**
- Pass learned knowledge to offspring
- Cultural transmission
- Evolution of strategies

**Complex Environment:**
- Seasons, weather, disasters
- Multiple biomes
- Predators or threats

**Tool Creation:**
- Agents invent tools
- Technology tree discovery
- Innovation diffusion

**Longer Timescales:**
- Multi-generational (if reproduction added)
- 10,000+ simulation days
- Cultural evolution

**Advanced Adapter System:**
- More specialized adapters
- Adapter inheritance between agents
- Meta-learning adapters

---

## 🤔 Open Questions

**Scientific:**
- Is cooperation inevitable given intelligence + scarcity?
- What's the minimal cognitive architecture for social behavior?
- Are human institutions (property, trade, law) universal or cultural?

**Technical:**
- Can 1B models develop genuine creativity?
- How much knowledge can be learned vs. must be pre-programmed?
- What's the optimal LoRA rank for different adapter types?
- How do System 1/2 ratios evolve over time?

**Philosophical:**
- What does this tell us about consciousness?
- Is learned knowledge "real" understanding?
- Are we in a simulation? (just kidding... or are we?)

---

## 📚 References & Inspirations

**Academic Research:**
- "Generative Agents: Interactive Simulacra of Human Behavior" (Stanford, 2023)
- Game theory and emergence of cooperation (Axelrod)
- Multi-agent reinforcement learning literature
- Continual learning in neural networks
- **LoRA: Low-Rank Adaptation of Large Language Models (Hu et al., 2021)**
- **Dual-process theory (Kahneman, System 1 and System 2)**

**Differences from Existing Work:**
- Stanford's Smallville uses GPT-4 (pre-loaded with human knowledge)
- Most RL agents can't reason abstractly
- No existing work combines: blank slate + reasoning + continuous learning

**This project's novelty:**
- True minimal knowledge starting point
- Experience-based knowledge building
- Tests genuine emergence vs. pattern matching
- **NEW: LoRA adapters for catastrophic forgetting prevention**
- **NEW: Dual-process architecture (Lizard Brain + LLM)**
- **NEW: Biologically-inspired sleep learning**

---

## 🚀 Getting Started

### Quick Start (Week 1)

1. **Set up development environment**
   ```bash
   git clone [your-repo]
   cd project-genesis
   pip install -r requirements.txt
   pip install peft  # For LoRA
   ```

2. **Run simple test simulation**
   ```bash
   python test_world.py
   # Should see: 50x50 grid, 2 agents, basic survival
   # Watch for 🟢/🔵 mode indicators
   ```

3. **Download/prepare base model**
   ```bash
   python prepare_model.py --size 1B --filter-dataset
   # Will take 1-2 weeks to fully train
   ```

4. **Test Lizard Brain independently**
   ```bash
   python test_lizard_brain.py --days 50
   # Agent should survive on instincts alone
   ```

5. **Run first experiment**
   ```bash
   python run_simulation.py --days 100 --agents 2
   # Observe behaviors, check logs
   # Watch day/night cycles
   ```

### Join the Project

**This is ambitious. You don't have to do it alone.**

Consider:
- Open sourcing on GitHub
- Recruiting collaborators
- Seeking academic advisors
- Applying for research grants
- Documenting as you build (blog/videos)

---

## ✅ Summary: The Path Forward

**What You're Building:**
Two AI agents with minimal starting knowledge, placed in a simulated Earth, who learn ONLY from experience through continuous LoRA adapter training during "sleep" cycles, with dual-process architecture ensuring they never freeze.

**Key Design Decisions:**
1. **🌙 Sleep Learning:** Training happens at "night," no simulation interruption
2. **🧬 LoRA Adapters:** Base model frozen, tiny adapters learn (no forgetting)
3. **🦎 Lizard Brain:** Hardcoded instincts ensure agents always act

**Why It's Hard:**
Combines blank-slate learning, complex reasoning, and emergent social behavior - nobody has done this exact approach.

**Why It's Feasible:**
Small models (1B), LoRA adapters (fast training), Lizard Brain (guaranteed survival), selective retraining (only significant experiences), your available hardware.

**Why It Matters:**
Tests whether human civilization is inevitable or contingent. Could reveal universal principles of cooperation, or show us we could have done things completely differently.

**Timeline:**
6-9 months to meaningful results, 12+ months for deep exploration.

**The Real Goal:**
Not to perfectly simulate human civilization, but to understand what emerges when intelligence meets scarcity without cultural templates.

---

## 💬 Final Thoughts

This project asks one of the most profound questions in AI and philosophy: **What is inevitable about civilization, and what is accident?**

Whether you discover that agents recreate human-like structures (suggesting inevitability) or build something completely alien (suggesting contingency), both answers are fascinating.

The journey of building this - even if it "fails" - will teach you more about AI, emergence, and intelligence than most formal education.

**Start small. Learn fast. Iterate often. Document everything.**

And remember: the most interesting results are often the unexpected ones.

---

**Project Status:** Concept & Design Phase  
**Next Milestone:** Base model training + Lizard Brain implementation  
**Timeline:** 6-9 months to first results  
**Last Updated:** November 2024

---

*"The question is not whether we can build artificial intelligence that thinks like us, but whether intelligence itself inevitably leads to certain forms of cooperation, communication, and civilization."*
