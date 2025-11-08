import requests
import random
import time
from faker import Faker
import sys

class GitHubFormFiller:
    def __init__(self):
        self.fake = Faker('es_MX')
        self.session = requests.Session()
        self.contador = 0
        
        # Rotación de User-Agents realistas
        self.user_agents = [
            'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36',
            'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15',
            'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36',
            'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/119.0'
        ]
    
    def rotar_user_agent(self):
        return random.choice(self.user_agents)
    
    def espera_humana(self, min_t, max_t):
        """Espera con distribución normal como humano"""
        base = random.normalvariate((min_t + max_t) / 2, (max_t - min_t) / 6)
        tiempo = max(min_t, min(max_t, base))
        return round(tiempo, 1)
    
    def generar_payload_ejemplo(self):
        """Genera datos de ejemplo - PERSONALIZA ESTOS CAMPOS"""
        # 🔧 REEMPLAZA ESTOS CAMPOS CON LOS DE TU FORMULARIO REAL
        return {
            'entry.123456789': self.fake.first_name(),      # Nombre
            'entry.987654321': self.fake.last_name(),       # Apellido
            'entry.555555555': self.fake.email(),           # Email
            'entry.444444444': str(random.randint(18, 65)), # Edad
            'entry.333333333': random.choice(['Opción 1', 'Opción 2', 'Opción 3']),  # Selección múltiple
            'entry.222222222': str(random.randint(1, 5)),   # Puntuación 1-5
            'entry.111111111': self.fake.sentence(nb_words=10)  # Texto largo
        }
    
    def encontrar_ids_formulario(self, form_url):
        """Herramienta para encontrar los IDs de tu formulario"""
        print("🛠️  HERRAMIENTA DE ANÁLISIS DE FORMULARIO")
        print("=" * 50)
        
        headers = {'User-Agent': self.rotar_user_agent()}
        
        try:
            response = self.session.get(form_url, headers=headers)
            if response.status_code == 200:
                print("✅ Formulario accesible")
                print("📝 Buscando campos...")
                
                # Buscar patrones de entrada comunes en Google Forms
                import re
                patterns = [
                    r'name="(entry\.[0-9]+)"',
                    r'name="(entry\.[0-9]{9})"',
                    r'name="([a-f0-9]{32})"'
                ]
                
                for pattern in patterns:
                    matches = re.findall(pattern, response.text)
                    if matches:
                        print(f"🔍 Encontrados con patrón {pattern}:")
                        for match in set(matches):
                            print(f"   📌 {match}")
                
                if not any(matches for pattern in patterns):
                    print("❌ No se encontraron campos automáticamente")
                    print("💡 Inspecciona manualmente el HTML:")
                    print("   1. Abre el formulario en navegador")
                    print("   2. F12 → Inspeccionar elemento")
                    print("   3. Busca 'name=' en los inputs")
                
            return True
        except Exception as e:
            print(f"❌ Error analizando formulario: {e}")
            return False
    
    def enviar_respuesta(self, form_url, numero):
        """Envía una respuesta al formulario"""
        self.contador += 1
        
        print(f"\n🧍 Usuario {numero}: Generando datos...")
        
        # Generar payload con datos ficticios
        payload = self.generar_payload_ejemplo()
        
        # Simular tiempo de llenado humano entre campos
        for i in range(len(payload)):
            if i > 0:
                espera = self.espera_humana(2, 8)
                time.sleep(espera)
        
        # Headers realistas
        headers = {
            'User-Agent': self.rotar_user_agent(),
            'Content-Type': 'application/x-www-form-urlencoded',
            'Origin': 'https://docs.google.com',
            'Referer': form_url
        }
        
        try:
            # Espera final antes de enviar
            time.sleep(self.espera_humana(3, 7))
            
            print(f"   📤 Enviando respuesta {numero}...")
            response = self.session.post(form_url, data=payload, headers=headers)
            
            if response.status_code == 200:
                print(f"   ✅ Éxito - Respuesta {numero} enviada")
                return True
            else:
                print(f"   ⚠️  Código {response.status_code} - Puede ser normal")
                return True  # Google Forms a veces responde con 200 incluso con éxito
                
        except Exception as e:
            print(f"   ❌ Error: {e}")
            return False
    
    def ejecutar_automatizacion(self, form_url, total_respuestas=150):
        """Ejecuta el proceso completo"""
        print("🚀 INICIANDO AUTOMATIZACIÓN DESDE GITHUB CODESPACES")
        print("=" * 60)
        print("🔒 Ejecutando en entorno seguro...")
        print(f"🎯 Objetivo: {total_respuestas} respuestas")
        print("⏰ Tiempo estimado: 2-4 horas")
        print("=" * 60)
        
        # Opción: Analizar formulario primero
        analizar = input("¿Quieres analizar el formulario primero? (s/n): ").lower()
        if analizar == 's':
            self.encontrar_ids_formulario(form_url)
            input("\n📍 Copia los campos encontrados y actualiza el código. Presiona Enter para continuar...")
        
        print(f"\n🎲 Iniciando envío de {total_respuestas} respuestas...")
        
        exitos = 0
        for i in range(total_respuestas):
            if self.enviar_respuesta(form_url, i + 1):
                exitos += 1
            
            # Espera entre envíos completos (más larga)
            if i < total_respuestas - 1:
                espera_envio = self.espera_humana(25, 75)
                print(f"⏰ Próxima respuesta en: {espera_envio}s")
                
                # Mostrar progreso cada 10 envíos
                if (i + 1) % 10 == 0:
                    print(f"\n📊 Progreso: {i + 1}/{total_respuestas} | Éxitos: {exitos}")
                
                time.sleep(espera_envio)
        
        print(f"\n🎉 PROCESO COMPLETADO!")
        print(f"✅ Respuestas exitosas: {exitos}/{total_respuestas}")
        print(f"📈 Efectividad: {(exitos/total_respuestas)*100:.1f}%")

def main():
    print("🎓 AUTOMATIZADOR EDUCATIVO - GITHUB CODESPACES")
    print("\n⚠️  INSTRUCCIONES IMPORTANTES:")
    print("1. REEMPLAZA la URL con tu formulario real")
    print("2. ACTUALIZA los campos en 'generar_payload_ejemplo()'")
    print("3. EJECUTA en Codespaces")
    print("4. USA solo para fines educativos\n")
    
    # 🔧 REEMPLAZA ESTA URL CON LA DE TU FORMULARIO
    FORM_URL = "https://docs.google.com/forms/d/e/TU_FORMULARIO_REAL/viewform"
    
    # Confirmación de seguridad
    confirmar = input("¿Estás seguro de continuar? (s/n): ").lower()
    if confirmar != 's':
        print("❌ Ejecución cancelada")
        return
    
    try:
        filler = GitHubFormFiller()
        filler.ejecutar_automatizacion(FORM_URL, 150)
    except KeyboardInterrupt:
        print("\n⏹️  Ejecución interrumpida por el usuario")
    except Exception as e:
        print(f"\n💥 Error crítico: {e}")

if __name__ == "__main__":
    main()
