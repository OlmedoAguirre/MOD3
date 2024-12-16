from pymongo import MongoClient
from bson.objectid import ObjectId


def conectar_db(uri="mongodb://localhost:27017", db_name="librorecetas"):
    try:
        cliente = MongoClient(uri)
        db = cliente[db_name]
        return db
    except Exception as e:
        print("Error al conectar a la base de datos: " + str(e))
        return None


def agregar_receta(db):
    nombre = input("Nombre de la receta: ")
    ingredientes = input("Ingredientes (separados por comas): ")
    pasos = input("Pasos (separados por comas): ")
    receta = {
        "nombre": nombre,
        "ingredientes": ingredientes,
        "pasos": pasos
    }
    try:
        resultado = db.recetas.insert_one(receta)
        print(f"Receta agregada con éxito. ID: {resultado.inserted_id}")
    except Exception as e:
        print("Error al agregar receta: " + str(e))


def actualizar_receta(db):
    ver_recetas(db)
    receta_id = input("Ingrese el ID de la receta a actualizar: ")
    try:
        receta = db.recetas.find_one({"_id": ObjectId(receta_id)})
        if receta:
            print("Ingrese los datos a cambiar (dejar en blanco para no cambiar):")
            nuevo_nombre = input(f"Nuevo nombre [{receta['nombre']}]: ") or receta['nombre']
            nuevos_ingredientes = input(f"Nuevos ingredientes [{receta['ingredientes']}]: ") or receta['ingredientes']
            nuevos_pasos = input(f"Nuevos pasos [{receta['pasos']}]: ") or receta['pasos']

            db.recetas.update_one(
                {"_id": ObjectId(receta_id)},
                {"$set": {"nombre": nuevo_nombre, "ingredientes": nuevos_ingredientes, "pasos": nuevos_pasos}}
            )
            print("Receta actualizada exitosamente.")
        else:
            print("Error: No se encontró una receta con ese ID.")
    except Exception as e:
        print("Error al actualizar la receta: " + str(e))


def eliminar_receta(db):
    ver_recetas(db)
    receta_id = input("Ingrese el ID de la receta a eliminar: ")
    try:
        resultado = db.recetas.delete_one({"_id": ObjectId(receta_id)})
        if resultado.deleted_count > 0:
            print("Receta eliminada exitosamente.")
        else:
            print("Error: No se encontró una receta con ese ID.")
    except Exception as e:
        print("Error al eliminar la receta: " + str(e))


def ver_recetas(db):
    try:
        recetas = db.recetas.find()
        if recetas.count() > 0:
            print("\nLista de recetas registradas:")
            for receta in recetas:
                print(f"ID: {receta['_id']} | Nombre: {receta['nombre']} | Ingredientes: {receta['ingredientes']} | Pasos: {receta['pasos']}")
            print()
        else:
            print("No hay recetas registradas.")
    except Exception as e:
        print("Error al mostrar las recetas: " + str(e))


def menu():
    print("\n--- Libro de Recetas ---")
    print("1) Agregar nueva receta")
    print("2) Actualizar receta")
    print("3) Eliminar receta")
    print("4) Ver recetas")
    print("5) Salir")


def main():
    db = conectar_db()
    if db is None:
        return
    while True:
        menu()
        opcion = input("Seleccione una opción: ").strip()
        if opcion == "1":
            agregar_receta(db)
        elif opcion == "2":
            actualizar_receta(db)
        elif opcion == "3":
            eliminar_receta(db)
        elif opcion == "4":
            ver_recetas(db)
        elif opcion == "5":
            print("Saliendo del programa...")
            break
        else:
            print("Opción inválida. Intente de nuevo.")


if __name__ == "__main__":
    main()
