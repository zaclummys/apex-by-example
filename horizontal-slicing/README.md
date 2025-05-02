# Horizontal Slicing

In a layered architecture, horizontal slicing refers to the practice of organizing code into layers that represent different levels of abstraction. Each layer is responsible for a specific aspect of the application, and they communicate with each other through well-defined interfaces.

This approach allows for better separation of concerns, making it easier to maintain and evolve the codebase over time. In this section, we will explore how to implement horizontal slicing in Apex, focusing on the following layers:
- **Presentation Layer**: This layer is responsible for handling outside interactions such as Lightning Web Components, Visualforce Pages and Flow. It receives requests, passes them to the application layer, and handles the responses.
- **Application Layer**: This layer contains the application logic and orchestrates the flow of data between the presentation layer and other layers. It coordinates the interactions between different services and handles cross-cutting concerns.
- **Domain Layer**: This layer represents the core business domain and contains the domain entities, the relationships between them, and domain services. It encapsulates the business logic and rules specific to the domain.
- **Infrastructure Layer**: This layer provides the technical capabilities and services needed to support the application, such as data access, external service integrations, and other technical concerns. It does not contain any business logic or domain-specific code.

## Presentation Layer

The presentation layer is responsible for handling interactions with the outside world, such as Lightning Web Components, Visualforce Pages, Flows and Inbound Callouts. It receives requests, passes them to the application layer, and handles the responses. The presentation layer should be thin and focused on translating user input into a format that can be understood by the application layer and vice versa. It is typically implemented as a `Controller`, `Handler` or `REST` class.

Let's say we have a simple real estate application that allows users to update the address of a property. The presentation layer would look like this:

```apex
public class UpdatePropertyAddressController {
    @AuraEnabled
    public static void execute (Id propertyId, String street, String city, String state, String zipCode) {
        UpdatePropertyAddressService.execute(propertyId, street, city, state, zipCode);
    }
}
```

Note that the presentation layer is responsible only for handling the request and passing it to the application layer. It does not contain any business logic or data access code.

## Application Layer

The application layer is responsible for orchestrating the flow of data between the presentation layer and other layers. It contains the application logic and coordinates the interactions between different services. It is typically implemented as a `Service`.


```apex
public class UpdatePropertyAddressService {
    public static void execute (Id propertyId, String street, String city, String state, String zipCode) {
        // Validate the input data
        if (String.isBlank(propertyId)) {
            throw new UpdatePropertyAddressServiceException('Property ID cannot be blank.');
        }

        // Retrieve the property from the infrastructure layer
        Property property = PropertyRepository.getById(propertyId);

        // Update the address in the domain layer
        property.updateAddress(street, city, state, zipCode);

        // Save the updated property back to the infrastructure layer
        PropertyRepository.save(property);
    }

    public class UpdatePropertyAddressServiceException extends Exception {}
}
```

This layer can also be used to implement feature toggling, caching, logging, and other cross-cutting concerns.

## Domain Layer
The domain layer is responsible for representing the core business domain and contains the domain entities, the relationships between them and domain services. It is typically implemented as a set of classes that represent the business objects and their behavior using a non-technical language.

The domain layer should not depend on any other layers, and it should contain the business logic that is specific to the domain. This allows us to keep the domain layer independent and focused on the core business rules.

Let's see how we can implement a `Property` with an `Address` in the domain layer:

```apex
public class Property {
    private Id id;
    private String name;
    private Address address;

    public Property (Id id, String name, String street, String city, String state, String zipCode) {
        this.id = id;
        this.name = name;
        this.address = new Address(street, city, state, zipCode);
    }

    public Id getId () {
        return id;
    }

    public String getName () {
        return name;
    }

    public String getStreet () {
        return address.getStreet();
    }

    public String getCity () {
        return address.getCity();
    }

    public String getState () {
        return address.getState();
    }

    public String getZipCode () {
        return address.getZipCode();
    }

    public void updateAddress(String street, String city, String state, String zipCode) {
        this.address = new Address(street, city, state, zipCode);
    }

    private class Address {
        private String street;
        private String city;
        private String state;
        private String zipCode;

        private Address (String street, String city, String state, String zipCode) {
            if (String.isBlank(street)) {
                throw new IllegalArgumentException('Street cannot be blank.');
            }

            if (String.isBlank(city)) {
                throw new IllegalArgumentException('City cannot be blank.');
            }

            if (String.isBlank(state)) {
                throw new IllegalArgumentException('State cannot be blank.');
            }

            if (String.isBlank(zipCode)) {
                throw new IllegalArgumentException('Zip code cannot be blank.');
            }

            this.street = street;
            this.city = city;
            this.state = state;
            this.zipCode = zipCode;
        }
    }
}
```

Notice that the `Property` class contains the business logic related to the property, such as updating the address. It also contains a nested `Address` class that represents the address of the property. The `Address` class is responsible for validating the address fields and ensuring that they are not blank.

The `Property` class is also hiding its implementation details, so other layers do not need to know about the internal structure of the `Property` class. This allows us to keep the domain layer focused on the business logic and can change more easily without affecting other layers.

## Infrastructure Layer
The infrastructure layer is responsible for providing the technical capabilities and services needed to support the application, such as data access, external service integrations, and other technical concerns. It does not contain any business logic or domain-specific code. It is typically implemented as a set of classes that provide technical services.

Some examples of classes that could be implemented in the infrastructure layer include:
- **Repositories**: Classes that handle data access and persistence. They interact with the database and provide methods to retrieve, save, and delete domain entities.
- **Callouts**: Classes that handle external service integrations. They provide methods to make Outbound callouts to external APIs and handle the responses.
- **Utilities**: Classes that provide technical services, such as logging, caching, and configuration management.

Now, let's see an example of a repository that retrieves and saves properties to the database.

```apex
public class PropertyRepository {
    public static Property getById (Id propertyId) {
        Property__c propertyRecord = [
            SELECT
                Id,
                Name,
                Street__c,
                City__c,
                State__c,
                ZipCode__c
            FROM Property__c
            WHERE Id = :propertyId
        ];

        Property property = new Property(
            propertyRecord.Id,
            propertyRecord.Name,
            propertyRecord.Street__c,
            propertyRecord.City__c,
            propertyRecord.State__c,
            propertyRecord.ZipCode__c
        );

        return property;
    }

    public static void save (Property property) {
        Property__c propertyRecord = new Property__c(
            Id = property.getId(),
            Name = property.getName(),
            Street__c = property.getStreet(),
            City__c = property.getCity(),
            State__c = property.getState(),
            ZipCode__c = property.getZipCode()
        );

        upsert propertyRecord;

        // Handle any additional logic needed after saving the property
    }
}
```