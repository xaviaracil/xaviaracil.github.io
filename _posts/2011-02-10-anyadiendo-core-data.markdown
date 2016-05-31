---
author: desarrolloenmac
comments: true
date: 2011-02-10 08:43:22+00:00
layout: single
slug: anyadiendo-core-data
title: Añadiendo Core Data
wordpress_id: 26
categories:
- Core Data
- iPhone
- XCode
---

Estoy desarrollando una aplicación para iOS (iPhone concretamente), para la cual he escogido el template de TabBarApplication. Para el modelo de datos he pensado en utilizar CoreData, pero el template escogido no lo trae por defecto.

[![](/images/2011-02-10-anyadiendo-core-data/tabbar_template-300x204.png)](/images/2011-02-10-anyadiendo-core-data/tabbar_template.png)

Para añadir Core Data a la aplicación lo más sencillo es seguir los pasos que indica el documento [Core Data Tutorial for iOS](http://developer.apple.com/library/ios/#documentation/DataManagement/Conceptual/iPhoneCoreData01/Introduction/Introduction.html%23//apple_ref/doc/uid/TP40008305-CH1-SW1). Lo primero que hay que hacer es añadir el framework Core Data al target. En Xcode 4 seleccionamos el proyecto, luego el target, click en la pestaña _Build Phases_ y en la fase _Link Binary With Libraries_ añadir CoreData.framework

[![](/images/2011-02-10-anyadiendo-core-data/add_coredata_framework-300x204.png)](/images/2011-02-10-anyadiendo-core-data/add_coredata_framework.png)

<!-- more -->
Seguidamente modificamos el fichero .pch para que todas las classes tengan el import de Core Data de forma automática. Más o menos quedaría así:


    //
    // Prefix header for all source files of the 'PruebaCoreData' target in the 'PruebaCoreData' project
    //
    #import <Availability.h>
    #ifndef __IPHONE_3_0
    	#warning "This project uses features only available in iPhone SDK 3.0 and later."
    #endif
    #ifdef __OBJC__
    	#import <UIKit/UIKit.h>
    	#import <Foundation/Foundation.h>
    	#import <CoreData/CoreData.h>
    #endif


La única línea que se ha añadido ha sido


    #import <CoreData/CoreData.h>


Una vez que tenemos el framework incluido en el proyecto, el siguiente paso es crear los objetos, Así, añado en el AppDelegate.h la definición de los objetos y métodos:


    #import <UIKit/UIKit.h>
    @interface PruebaCoreDataAppDelegate : NSObject <UIApplicationDelegate, UITabBarControllerDelegate> {
    	NSManagedObjectContext *managedObjectContext;
    	NSManagedObjectModel *managedObjectModel;
    	NSPersistentStoreCoordinator *persistentStoreCoordinator;
    }
    @property (nonatomic, retain) IBOutlet UIWindow *window;
    @property (nonatomic, retain) IBOutlet UITabBarController *tabBarController;
    @property (nonatomic, retain, readonly) NSManagedObjectContext *managedObjectContext;
    @property (nonatomic, retain, readonly) NSManagedObjectModel *managedObjectModel;
    @property (nonatomic, retain, readonly) NSPersistentStoreCoordinator *persistentStoreCoordinator;
    - (NSURL *)applicationDocumentsDirectory;
    - (void)saveContext;
    @end


Las propiedades relacionadas con Core Data son:




  * managedObjectContext, que contiene el contexto de Core Data, es decir, el conjunto de entidades existentes.


  * managedObjectModel, que contiene la información del modelo de datos, es decir, la definición de las entidades, sus atributos y las relaciones entre entidades.


  * persistentStoreCoordinator, que gestiona la persistencia del contexto en disco (en el caso de iOS normalmente suele ser una base de datos sqlite)


Los métodos relacionados con Core Data son:


  * - (NSURL *)applicationDocumentsDirectory, que devuelve la URL del directorio donde guardar la base de datos.


  * - (void)saveContext, que realiza la persistencia del contexto en base de datos.


La implementación es la siguiente:


    @implementation PruebaCoreDataAppDelegate
    ...
    @synthesize managedObjectContext;
    @synthesize managedObjectModel;
    @synthesize persistentStoreCoordinator;
    ...
    - (void)dealloc
    {
    	[managedObjectContext release];
    	[managedObjectModel release];
    	[persistentStoreCoordinator release];
    	...
    }
    ...
    - (void)saveContext
    {
    	NSError *error = nil;
    	NSManagedObjectContext *managedObjectContext = self.managedObjectContext;
    	if (managedObjectContext != nil)
    	{
    		if ([managedObjectContext hasChanges] && ![managedObjectContext save:&error])
    		{
    			/*
    			Replace this implementation with code to handle the error appropriately.
    			abort() causes the application to generate a crash log and terminate.
    			You should not use this function in a shipping application, although it may be useful during development.
    			If it is not possible to recover from the error, display an alert panel that instructs the user to quit
    			the application by pressing the Home button.
    			*/
    			NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
    			abort();
    			}
    	}
    }
    #pragma mark - Core Data stack
    /**
    Returns the managed object context for the application.
    If the context doesn't already exist, it is created and bound to the persistent store coordinator for the application.
    */
    - (NSManagedObjectContext *)managedObjectContext
    {
    	if (managedObjectContext != nil)
    	{
    		return managedObjectContext;
    	}
    	NSPersistentStoreCoordinator *coordinator = [self persistentStoreCoordinator];
    	if (coordinator != nil)
    	{
    		managedObjectContext = [[NSManagedObjectContext alloc] init];
    		[managedObjectContext setPersistentStoreCoordinator:coordinator];
    	}
    	return managedObjectContext;
    }
    /**
    Returns the managed object model for the application.
    If the model doesn't already exist, it is created from the application's model.
    */
    - (NSManagedObjectModel *)managedObjectModel
    {
    	if (managedObjectModel != nil)
    	{
    		return managedObjectModel;
    	}
    	NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"PruebaCoreData" withExtension:@"momd"];
    	managedObjectModel = [[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL];
    	return managedObjectModel;
    	}
    /**
    Returns the persistent store coordinator for the application.
    If the coordinator doesn't already exist, it is created and the application's store added to it.
    */
    - (NSPersistentStoreCoordinator *)persistentStoreCoordinator
    {
    	if (persistentStoreCoordinator != nil)
    	{
    		return persistentStoreCoordinator;
    	}
    	NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"PruebaCoreData.sqlite"];
    	NSError *error = nil;
    	persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:[self managedObjectModel]];
    	if (![persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:nil error:&error])
    	{
    		/*
    		Replace this implementation with code to handle the error appropriately.
    		abort() causes the application to generate a crash log and terminate. You should not use this function in a
    		shipping application, although it may be useful during development. If it is not possible to recover from the error,
    		display an alert panel that instructs the user to quit the application by pressing the Home button.
    		Typical reasons for an error here include:
    		* The persistent store is not accessible;
    		* The schema for the persistent store is incompatible with current managed object model.
    		Check the error message to determine what the actual problem was.
    		If the persistent store is not accessible, there is typically something wrong with the file path. Often, a file URL is
    		pointing into the application's resources directory instead of a writeable directory.
    		If you encounter schema incompatibility errors during development, you can reduce their frequency by:
    		* Simply deleting the existing store:
    		[[NSFileManager defaultManager] removeItemAtURL:storeURL error:nil]
    		* Performing automatic lightweight migration by passing the following dictionary as the options parameter:
    		[NSDictionary dictionaryWithObjectsAndKeys:[NSNumber numberWithBool:YES], NSMigratePersistentStoresAutomaticallyOption, [NSNumber numberWithBool:YES], NSInferMappingModelAutomaticallyOption, nil];
    		Lightweight migration will only work for a limited set of schema changes; consult "Core Data Model Versioning and Data Migration Programming Guide" for details.
    		*/
    		NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
    		abort();
    		}
    	return persistentStoreCoordinator;
    }
    #pragma mark - Application's Documents directory
    /**
    Returns the URL to the application's Documents directory.
    */
    - (NSURL *)applicationDocumentsDirectory
    {
    	return [[[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] lastObject];
    }


En el código hay dos líneas clave, donde se indican el nombre del fichero sqlite


    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"PruebaCoreData.sqlite"];


y donde se indica el fichero que contiene el modelo de datos


    NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"PruebaCoreData" withExtension:@"momd"];


El último paso es crear el modelo de datos. En Xcode es tan fácil como File->New File...

[![](/images/2011-02-10-anyadiendo-core-data/datamodel_template-300x204.png)](/images/2011-02-10-anyadiendo-core-data/datamodel_template.png)

[](/images/2011-02-10-anyadiendo-core-data/datamodel_template.png)El nombre tiene que ser el indicado en el código (en nuestro caso, PruebaCoreData).

[![](/images/2011-02-10-anyadiendo-core-data/modelo_datos-300x209.png)](/images/2011-02-10-anyadiendo-core-data/modelo_datos.png)

Y ya está. Los siguientes pasos son crear entidades y _jugar_ con ellas en el resto de la aplicación.

Un último consejo: Si antes de crear la entidad ejecutáis la aplicación en el simulador,  es muy probable que al acceder a la entidad os de un error de tipo


    Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: '+entityForName: could not locate an NSManagedObjectModel for entity name 'xxx'' .


Esto es porque el modelo de datos tiene versionado y no se actualiza hasta que la versión se modifique. La solución pasa por eliminar la aplicación del simulador y volver a ejecutarla
