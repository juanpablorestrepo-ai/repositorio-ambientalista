import discord
from discord.ext import commands
import random

# Configuración del bot con intents corregidos
intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="%", intents=intents)

# --- Comando de explicación ---
@bot.command()
async def contaminacion(ctx):
    mensaje = (
        "¿Qué es la contaminación?\n"
        "Es cuando entran cosas dañinas en un lugar donde no deberían estar y rompen el equilibrio.\n\n"
        "Tipos de contaminantes: físicos, químicos o biológicos que afectan agua, suelo o aire.\n\n"
        "Contaminación del aire: fábricas y carros meten gases tóxicos como monóxido de carbono y óxidos de azufre. "
        "Eso calienta el planeta más de lo normal y puede enfermarnos.\n\n"
        "Efecto invernadero: fenómeno natural que mantiene la Tierra calientica, pero con tanta contaminación "
        "ese 'abrigo' se vuelve demasiado grueso y el planeta se recalienta.\n\n"
        "Contaminación natural: no todo es culpa humana. Algas sueltan toxinas y volcanes lanzan cenizas y minerales.\n\n"
        "En resumen: la contaminación es meterle basura invisible al ambiente, y cuando se acumula demasiado, "
        "nos enferma y daña el planeta."
    )
    await ctx.send(mensaje)

# --- Comando de trivia ---
preguntas_trivia = [
    {
        "pregunta": "¿Cuál de estos gases es tóxico en altas concentraciones?",
        "opciones": ["A) Oxígeno", "B) Monóxido de carbono", "C) Nitrógeno"],
        "respuesta": "B"
    },
    {
        "pregunta": "¿Qué fenómeno natural también contamina?",
        "opciones": ["A) Volcanes", "B) Lluvia", "C) Viento"],
        "respuesta": "A"
    },
    {
        "pregunta": "¿Qué pasa si el efecto invernadero se vuelve muy fuerte?",
        "opciones": ["A) El planeta se enfría", "B) El planeta se recalienta", "C) No pasa nada"],
        "respuesta": "B"
    }
]

@bot.command()
async def trivia(ctx):
    pregunta = random.choice(preguntas_trivia)
    opciones = "\n".join(pregunta["opciones"])
    await ctx.send(f"{pregunta['pregunta']}\n{opciones}\n\nResponde con A, B o C.")

    def check(m):
        return m.author == ctx.author and m.content.upper().startswith(("A","B","C"))

    try:
        respuesta_usuario = await bot.wait_for("message", check=check, timeout=30)
        if respuesta_usuario.content.upper().startswith(pregunta["respuesta"]):
            await ctx.send(":D ¡Correcto! Eres un crack ambientalista.")
        else:
            await ctx.send(":( Ups, no es la respuesta. Pero tranqui, sigamos aprendiendo.")
    except:
        await ctx.send(":( Te demoraste mucho en responder, intenta otra vez.")

# --- Comando de completa la frase ---
frases_incompletas = [
    ("El aire normal tiene ___  ___", "nitrógeno y oxígeno"),
    ("Los contaminantes pueden ser ___, ___  ___", "físicos, químicos o biológicos"),
    ("El efecto invernadero mantiene la Tierra ___", "calientica para la vida")
]

@bot.command()
async def completa(ctx):
    frase, respuesta = random.choice(frases_incompletas)
    await ctx.send(f"Completa la frase: {frase}\n(Pista: piensa en lo que aprendiste)")
    
    def check(m):
        return m.author == ctx.author

    try:
        respuesta_usuario = await bot.wait_for("message", check=check, timeout=30)
        if all(palabra in respuesta_usuario.content.lower() for palabra in respuesta.lower().split()):
            await ctx.send(":D ¡Bien hecho! Esa era la respuesta.")
        else:
            await ctx.send(f":( No era esa. La respuesta correcta es: {respuesta}")
    except:
        await ctx.send(":( Te demoraste mucho en responder, intenta otra vez.")

# --- Comando de cuidados ---
@bot.command()
async def cuidados(ctx):
    mensaje = (
        "Cuidar el ambiente no es solo reciclar, es un estilo de vida:\n\n"
        "- **Agua:** cerrar la llave mientras te cepillas, reparar fugas y usar solo lo necesario.\n"
        "- **Energía:** apagar luces y aparatos que no uses, preferir bombillos LED.\n"
        "- **Residuos:** separar orgánicos, plásticos, vidrio y papel para que puedan reciclarse.\n"
        "- **Transporte:** caminar, usar bici o transporte público para reducir emisiones.\n"
        "- **Consumo:** elegir productos locales y evitar plásticos de un solo uso.\n\n"
        "Cada acción pequeña suma y ayuda a mantener el planeta sano."
    )
    await ctx.send(mensaje)

# --- Comando de fauna ---
@bot.command()
async def fauna(ctx):
    mensaje = (
        "La fauna es la vida animal que habita nuestro planeta. Cada especie cumple un rol vital:\n\n"
        "- **Polinizadores** como abejas y murciélagos ayudan a que las plantas se reproduzcan.\n"
        "- **Depredadores** mantienen el equilibrio controlando poblaciones de otras especies.\n"
        "- **Animales marinos** como peces y corales sostienen la cadena alimenticia en los océanos.\n\n"
        "Proteger la fauna significa conservar la biodiversidad y asegurar que los ecosistemas sigan funcionando. "
        "Cuando una especie desaparece, se rompe un eslabón de la cadena natural."
    )
    await ctx.send(mensaje)

# --- Comando de flora ---
@bot.command()
async def flora(ctx):
    mensaje = (
        "La flora son todas las plantas y árboles que nos rodean. Son esenciales para la vida:\n\n"
        "- **Producción de oxígeno:** gracias a la fotosíntesis, las plantas nos permiten respirar.\n"
        "- **Alimento y medicina:** muchas frutas, verduras y hierbas provienen de la flora.\n"
        "- **Protección del suelo:** las raíces evitan la erosión y mantienen fértil la tierra.\n"
        "- **Clima:** los bosques regulan la temperatura y almacenan carbono, frenando el cambio climático.\n\n"
        "Cuidar la flora es plantar árboles, proteger bosques y respetar las áreas verdes. "
        "Sin flora, la vida en la Tierra no sería posible."
    )
    await ctx.send(mensaje)

# --- Evento de inicio ---
@bot.event
async def on_ready():
    print(f"Bot Ambientalista conectado como {bot.user}")
    for guild in bot.guilds:
        canal = guild.text_channels[0]
        saludo = (
            "¡Hola! Soy tu Bot Ambientalista :D\n"
            "Me acabo de conectar y estoy listo para hablar contigo.\n\n"
            "Mi misión es enseñarte, jugar y motivarte a cuidar el planeta de una forma divertida.\n"
            "Yo te hablo directo: si quieres aprender más, solo dime uno de estos comandos:\n\n"
            "- %contaminacion -> Te explico qué es y cómo afecta.\n"
            "- %trivia -> Te lanzo preguntas rápidas, responde con A, B o C.\n"
            "- %completa -> Te doy frases incompletas para que las termines. con las frases -nitrógeno y oxógeno- -físicos, químicos o biológicos- -calientica para la vida\n"
            "- %cuidados -> Consejos prácticos y detallados para cuidar el ambiente.\n"
            "- %fauna -> Información más profunda sobre los animales y su rol.\n"
            "- %flora -> Explicación sobre las plantas y su importancia.\n\n"
            "Yo estoy aquí para que aprendas y te diviertas. ¡Vamos a darle!"
        )
        await canal.send(saludo)
        break

# Corre el bot 
bot.run(("token"))

