# ESL-Friendly Reference — LLM response idioms

Table of expressions that appear in AI assistant responses, with literal B2 replacements and Spanish hints. Seeded from real session transcripts (nullclaw → zeroclaw migration session, GLM/DeepSeek models).

## Idioms

| Expression | Literal meaning | Spanish hint |
|---|---|---|
| half right | partially correct | a medias bien |
| where things stand | the current status | en qué punto está todo |
| on the same page | agreeing / sharing the same understanding | de acuerdo |
| out of the box | without extra setup or configuration | sin configurar nada |
| up and running | working / started successfully | funcionando |
| the bottom line | the final result / the key point | al final, lo importante |
| long story short | to summarize | resumiendo |
| the gist | the main idea | la idea principal |
| under the hood | in the internal implementation | por dentro, internamente |
| ground truth | the real, verified data | la fuente verdadera |
| a grain of salt | with healthy doubt | con desconfianza |
| low-hanging fruit | the easiest tasks | lo más fácil primero |
| a heads-up | an advance warning | aviso previo |
| in the weeds | lost in minor details | perdido en detalles |
| ball-park figure | an approximate number | un número aproximado |

## Phrasal verbs

| Expression | Literal meaning | Spanish hint |
|---|---|---|
| pick up where X left off | continue from the previous point | continuar donde quedó |
| figure out | understand / find the cause | descubrir, entender |
| sort out | resolve / organize | resolver |
| look into | investigate | investigar |
| flag | mention as important | señalar |
| boil down to | mean, in essence | se reduce a |
| come down to | depend ultimately on | depende de |
| worth flagging / knowing | important to mention / know | vale la pena señalar |
| point out | mention / draw attention to | señalar |
| end up | finally do / become | terminar haciendo |
| run into | encounter (a problem) | encontrarse con |
| set up | configure / install | configurar |
| break down | stop working / fail | fallar; also: analizar por partes |
| drill down | examine in more detail | profundizar |
| weed out | remove the bad ones | eliminar los malos |
| iron out | resolve small problems | pulir detalles |
| nail down | fix precisely / confirm | fijar con precisión |
| zero in on | focus on | enfocarse en |
| carry out | perform / execute | ejecutar |
| back up | support / make a copy | respaldar |
| hand off | transfer to someone else | pasarle a otro |

## Metaphors in technical speech

| Expression | Literal meaning | Spanish hint |
|---|---|---|
| the config dance | the complicated config routine | la rutina complicada de config |
| hijack | take control by force | tomar el control |
| traps / pitfalls | common mistakes that cause problems | trampas / errores comunes |
| green (test) | passed | pasó, exitoso |
| this cost a cycle | this caused one extra cycle | esto hizo perder un ciclo |
| disappears / is gone | no longer exists / was removed | ya no existe / se eliminó |
| chatty (logs) | produces too much output | verboso |
| noisy | full of irrelevant output | con ruido, poco limpio |
| zombie process | a process that should be dead but isn't | proceso muerto que sigue vivo |
| drop-in replacement | a substitute needing no other changes | reemplazo directo |
| kitchen sink | everything included | todo incluido |
| shotgun approach | trying everything without focus | sin estrategia |
| silver bullet | one simple complete solution | bala de plata: solución mágica |
| monkeypatch | a quick fix applied at runtime | parche rápido |
| dogfood | use your own product | usar tu propio producto |
| bake in | include permanently | incluir de fábrica |

## False friends and traps for Spanish speakers

| Word | Looks like | Actually means |
|---|---|---|
| actually | actualmente | in fact / really |
| eventually | eventualmente | finally, in the end |
| assist | asistir | to help |
| assist (event) | asistir | to attend |
| fabric | fábrica | cloth / (technical) infrastructure (e.g. "the network fabric") |
| library | librería | library (librería = bookshop) |
| once | onze / once | when (after doing X) — very common in technical docs: "once the build finishes…" |
| introduce | introducir | to present / add (a change) |
| realize | realizar | to notice / understand |
| submit | someter | to send (a form, a PR) |
| support (verb) | soportar | to support = incluir soporte para (soportar = to tolerate) |

## Keep as-is (standard technical usage)

Do not rewrite these even though they are metaphorical in origin — they are the standard vocabulary a B2 technical reader already knows:

`boot, spawn, kill (a process), daemon, watch, mount, bind, fork, merge, commit, push, pull, hook, trigger, fire (an event), listen (on a port), serve, stream, cache, flush, garbage-collect, sandbox, scaffold, dry-run, smoke test`
