---
date: 2011-02-21 21:34:03+00:00
slug: usando-categorias-para-evitar-duplicar-codigo
title: Usando categorías para evitar duplicar código
wordpress_id: 54
categories:
- Cocoa
- Objective-C
- XCode
tags:
- Eclipse
- Java
- JDK
- NetBeans
---

La aplicación iOS que estoy desarrollando sigue su curso (por cierto, tendréis noticias de ella en [http://www.xadsolutions.com](http://www.xadsolutions.com)).


En ella tengo dos tipos de UIViewController que muestran el mismo NSManagedObject de varias formas diferentes: Varios IUTableViewController para mostrar datos en una tabla y un UIViewController que contiene un MKMapView con un mapa. Ambos llaman a un UIViewController para mostrar más información sobre el objeto seleccionado.

El problema es que no quiero duplicar el código para crear, inicializar y mostrar el mismo UIController en varios lugares diferentes: Ahí es donde una categoría entra en acción.


<!-- more -->


Las categorías son una manera en Objective C para añadir funcionalidad a una clase sin tener que hacer una subclase. Esto puede parecer extraño, ya que el concepto no existe en lenguages como Java. Pero tiene su utilidad, sobre todo en el caso que nos ocupa.

Como todo, tiene sus ventajas e inconvenientes. Personalmente, el **añadir métodos a clases existentes sin necesidad de hacer una subclase** es una ventaja considerable. Por ejemplo, podemos añadir métodos a clases comunes como NSString o NSArray mediante una categoría, y **automáticamente** estos métodos estan disponibles para todos los objetos de esa clase en la aplicación. De hecho, en tiempo de ejecución no hay diferencia entre una método _interno_ y otro definido en una categoría.

El incoveniente principal es que una categoria **sólo permite añadir métodos** a una clase, no atributos. Este aspecto entra en el ejemplo dado.




[![](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_mapa-159x300.png)](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_mapa.png)[![](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_tabla-159x300.png)](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_tabla.png)



Volvamos al ejemplo: Tenemos un UITableViewController y un UIViewController que en algun momento tienen que crear, inicializar y mostrar otro UIViewController. El código del método que crea, inicializa y muestra el UIViewController _detail_ es el siguiente:



    -(void) showDetailForCameraAtIndex:(NSInteger) index
    {
        // Navigation logic may go here. Create and push another view controller.
        CameraDetailViewController *detailViewController = [[CameraDetailViewController alloc] initWithNibName:@"CameraDetail" bundle:nil];

        NSArray *array = [self camerasArray];
        Camera *theCamera = [array objectAtIndex:index];
        detailViewController.camera = theCamera;

        // Pass the selected object to the new view controller.
        [self.navigationController pushViewController:detailViewController animated:YES];
        [detailViewController release];
    }


Como podemos ver, creamos un objeto CameraDetailViewController al que asignamos como property camera un objeto de nuestro array camerasArray. Un vez asignado, lo añadimos a la pila de nuestro UINavigationController.

Éste es el código que queremos llamar desde los dos UIViewControllers anteriores, uno cuando el usuario seleccionar una fila de la tabla y el otro cuando el usuario selecciona un elemento del mapa. Si miramos las definiciones de UITableViewController y UIViewController vemos que:




  * UITableViewController hereda de UIViewControllers : UIResponder : NSObject


  * UIViewControllers hereda de UIResponder : NSObject


Como podemos ver, UIViewControllers es la clase común de los dos, el lugar perfecto para añadir el método.

Para crear la categoria en XCode seleccionamos File -> New -> New File...

[![](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_1-300x204.png)](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_1.png)

Seleccionamos Objective-C category y Next

[![](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_2-300x204.png)](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_2.png)

Indicamos que queremos hacer una categoria sobre UIViewController y Next

[![](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_3-300x251.png)](/images/2011-02-21-usando-categorias-para-evitar-duplicar-codigo/categorias_new_3.png)

Seleccionamos el nombre del fichero (la convención es _clase+categoria_) y Save. XCode nos crea los ficheros UIViewController+Detail.h y UIViewController+Detail.m. El código es el siguiente:


    #import

    @interface UIViewController (UIViewController_Detail)

    @end


y para la implementación


    #import "UIViewController+Detail.h"

    @implementation UIViewController (UIViewController_Detail)

    @end


Añadimos los métodos como si fuera una clase normal y corriente. El código de UIViewController+Detail.h sería:


    #import

    @interface UIViewController (UIViewController_Detail)
    -(void) showDetailForCameraAtIndex:(NSInteger) index;
    -(IBAction)showCamera:(id) sender;
    @end


Defino dos métodos porque en el UITableViewController accedemos por índice mientras que en el UIViewController con el mapa accedemos por la action de un UIButton. La implementación es la que siguie


    #import "UIViewController+CamerasArray.h"
    #import "CameraDetailViewController.h"
    #import "Camera.h"

    @implementation UIViewController (UIViewController_Detail)
    - (IBAction) showCamera:(id) sender
    {
        UIButton *button = (UIButton *) sender;
        [self showDetailForCameraAtIndex:button.tag];
    }

    -(void) showDetailForCameraAtIndex:(NSInteger) index
    {
        if ([self respondsToSelector:@selector(camerasArray)] == NO) {
            return;
        }

        // Navigation logic may go here. Create and push another view controller.
        CameraDetailViewController *detailViewController = [[CameraDetailViewController alloc] initWithNibName:@"CameraDetail" bundle:nil];

        NSArray *array = [self performSelector:@selector(camerasArray)];

        Camera *theCamera = [array objectAtIndex:index];
        detailViewController.camera = theCamera;

        // Pass the selected object to the new view controller.
        [self.navigationController pushViewController:detailViewController animated:YES];
        [detailViewController release];
    }

    @end


Hay un par de claves en el código: El UIButton guarda en su property tag el índice del elemento en el array. Además, para evitar que XCode nos dé un warning al acceder al array (recordemos que no podemos definir atributos en una categoría), se accede a ella mediante


        NSArray *array = [self performSelector:@selector(camerasArray)];


De esta manera, lo único que tenemos que hacer en los UIViewController defeinir una property camerasArray. Para protegernos que llamar métodos no implementados (por ejemplo, en otro UIViewController que no tenga la propiedad camerasArray definida) comprobamos que el método existe mediante


        if ([self respondsToSelector:@selector(camerasArray)] == NO) {
            return;
        }


Y ya está. A partir de este momento todos los UIViewController de la aplicación tienen estos dos métodos definidos. Para usarlos, se importa el UIViewController+Detail.h en el fichero correspondiente y ya se puede llamar a los métodos. Como ejemplo dejo el método relacionado en el UITableViewController


    #pragma mark - Table view delegate
    - (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
    {
        [self showDetailForCameraAtIndex:indexPath.row];
    }


Ha sido mi primer uso real de las categorías y no puedo estar más contento. He intentado solucionar el tema del camerasArray con algún tipo de @protocol, pero sin éxito. Si alguien tiene alguna solución, por favor estaría muy interesado. Gracias!
