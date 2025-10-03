# ============================================================
# HONEYPOT COMPOUND — FINAL FORM
# ============================================================
# Author: Calvin (Illumighost)
# Philosophy: Recursive Nightmare + Adaptive Deception + Legacy
# Features:
#   - Adaptive, self-morphing decoys (unkillable honeypot)
#   - Randomized timers & conditional stalls
#   - Simulated open-port triggers & scanners
#   - Full structured logging (file + console + JSON)
#   - Glyph archive (poetic forensic record)
#   - Compound metaphor: compound (infrastructure),
#     dog (sentinel), girl (social lure), baby (legacy)
# ============================================================

import time, random, hashlib, json, logging
from logging.handlers import RotatingFileHandler
from dataclasses import dataclass, field

# -----------------------------
# Logging Setup
# -----------------------------
logger = logging.getLogger("honeypot")
logger.setLevel(logging.INFO)

file_handler = RotatingFileHandler("honeypot.log", maxBytes=5_000_000, backupCount=5)
console_handler = logging.StreamHandler()

formatter = logging.Formatter("%(asctime)s [%(levelname)s] %(message)s")
file_handler.setFormatter(formatter)
console_handler.setFormatter(formatter)

logger.addHandler(file_handler)
logger.addHandler(console_handler)

# -----------------------------
# Simulation Primitives
# -----------------------------

@dataclass
class SimPort:
    port: int
    service: str
    open: bool = False
    volatility: float = 0.1

    def tick(self):
        if random.random() < self.volatility:
            self.open = not self.open
        return self.open


@dataclass
class SimHost:
    ip: str
    persona: str
    ports: dict = field(default_factory=dict)

    def add_port(self, port_num: int, service: str, open=False, volatility=0.1):
        self.ports[port_num] = SimPort(port=port_num, service=service, open=open, volatility=volatility)

    def tick(self):
        changes = []
        for p in self.ports.values():
            prev = p.open
            now = p.tick()
            if prev != now:
                changes.append((p.port, now))
        return changes

    def scan(self):
        return {p.port: p.open for p in self.ports.values()}

    def morph(self):
        personas = ["ubuntu-apache", "windows-rdp", "k8s-api", "s3-bucket", "mysql-node"]
        self.persona = random.choice(personas)
        return self.persona


# -----------------------------
# Glyph Archive
# -----------------------------

class Ledger:
    def __init__(self):
        self.entries = []

    def add(self, depth, tag, data=None):
        entry = {"depth": depth, "tag": tag, "data": data}
        self.entries.append(entry)
        logger.info(json.dumps(entry))

    def dump(self):
        for e in self.entries:
            print(e)


# -----------------------------
# Poem Conditions (Philosophy)
# -----------------------------

def night_speaks(topic="hollows"):
    return topic == "hollows" and random.random() < 0.55

def nile_is_dry():
    return random.random() < 0.35

class Earth:
    def __enter__(self): return "soil"
    def __exit__(self, exc_type, exc, tb): return False

def all_is_lost():
    return random.random() < 0.2


# -----------------------------
# Compound Metaphors
# -----------------------------
# Dog = Sentinel (loyal watcher)
# Girl = Social lure (human vector)
# Baby = Legacy (continuity, future)

class SentinelDog:
    def bark(self, depth):
        logger.info(json.dumps({"depth": depth, "sentinel": "dog_bark"}))
        return "dog::loyal"

class SocialLure:
    def whisper(self, depth):
        logger.info(json.dumps({"depth": depth, "lure": "girl_whisper"}))
        return "girl::whisper"

class LegacyBaby:
    def cry(self, depth):
        logger.info(json.dumps({"depth": depth, "legacy": "baby_cry"}))
        return "baby::cry"


# -----------------------------
# Honeypot Compound Engine
# -----------------------------

class HoneypotCompound:
    def __init__(self, hosts, max_depth=20):
        self.hosts = hosts
        self.max_depth = max_depth
        self.ledger = Ledger()
        self.dog = SentinelDog()
        self.girl = SocialLure()
        self.baby = LegacyBaby()

    def run(self, depth=0):
        if depth > self.max_depth:
            self.ledger.add(depth, "signal", "dissolved")
            return

        # Random jitter
        time.sleep(random.uniform(0.05, 0.25))

        # Conditional stall
        if depth > 0 and depth % 7 == 0:
            time.sleep(random.uniform(0.5, 1.5))
            self.ledger.add(depth, "conditional", "stall")

        # --- Original Poem as Honeypot Clauses ---
        if night_speaks("hollows"):
            self.ledger.add(depth, "hollows", "bare")
        else:
            self.ledger.add(depth, "glyphs", "willows_wisp")

        if nile_is_dry():
            self.ledger.add(depth, "man", "no_notice")
        else:
            self.ledger.add(depth, "river", "memory")

        with Earth() as soil:
            self.ledger.add(depth, "intent", "planted")

        if not all_is_lost():
            self.ledger.add(depth, "rent", "paid")
        else:
            self.ledger.add(depth, "collapse", "owed")

        # --- Compound Metaphors ---
        self.ledger.add(depth, "sentinel", self.dog.bark(depth))
        self.ledger.add(depth, "lure", self.girl.whisper(depth))
        self.ledger.add(depth, "legacy", self.baby.cry(depth))

        # --- Kitchen Sink Additions ---
        for host in self.hosts:
            persona = host.morph()
            self.ledger.add(depth, "persona", persona)

            changes = host.tick()
            for port_num, now_open in changes:
                self.ledger.add(depth, "port_flip", f"{host.ip}:{port_num}:{'open' if now_open else 'closed'}")

            snapshot = host.scan()
            for port_num, is_open in snapshot.items():
                if is_open and port_num in (22, 80, 443, 3306, 6379):
                    self.ledger.add(depth, "trigger", f"{host.ip}:{port_num}")
                    time.sleep(random.uniform(0.05, 0.2))

            self.ledger.add(depth, "checksum", hashlib.sha256(f"{depth}{host.ip}".encode()).hexdigest()[:8])
            self.ledger.add(depth, "echo", "recursive_nightmare")
            self.ledger.add(depth, "entropy", "tick")

        # Recursive descent
        self.run(depth + 1)


# -----------------------------
# Assembly
# -----------------------------

def build_hosts():
    h1 = SimHost(ip="10.0.0.11", persona="willow-hollow")
    h1.add_port(22, "ssh", open=True, volatility=0.15)
    h1.add_port(80, "http", open=True, volatility=0.1)

    h2 = SimHost(ip="10.0.0.23", persona="nile-memory")
    h2.add_port(443, "https", open=True, volatility=0.08)
    h2.add_port(3306, "mysql", open=False, volatility=0.3)

    h3 = SimHost(ip="10.0.0.34", persona="earth-intent")
    h3.add_port(8080, "http-alt", open=True, volatility=0.18)
    h3.add_port(53, "dns", open=True, volatility=0.05)

    return [h1, h2, h3]


# -----------------------------
# Invocation
# -----------------------------

if __name__ == "__main__":
    hosts = build_hosts()
    engine = HoneypotCompound(hosts, max_depth=20)
    engine.run()
    engine.ledger.dump()
