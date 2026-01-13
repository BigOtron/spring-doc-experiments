- Spring features a Validator interface that you can use to validate objects. The Validator interface works by using 
  an Errors object so that, while validating, validators can report validation failures to the Errors object.
```java
package spring.learn;

import org.jspecify.annotations.NonNull;
import org.springframework.stereotype.Component;
import org.springframework.validation.Errors;
import org.springframework.validation.ValidationUtils;
import org.springframework.validation.Validator;

@Component
public class UserValidator implements Validator {
    @Override
    public boolean supports(@NonNull Class<?> clazz) {
        return User.class.equals(clazz);
    }

    @Override
    public void validate(@NonNull Object target, @NonNull Errors errors) {
        ValidationUtils.rejectIfEmpty(errors, "name", "name.empty");
        User user = (User) target;
        if (user.getAge() < 0) {
            errors.rejectValue("age", "negative_value");
        } else if (user.getAge() > 110) {
            errors.rejectValue("age", "too_old");
        }
    }
}
```
```java
package spring.learn;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.stereotype.Service;
import org.springframework.validation.BeanPropertyBindingResult;
import org.springframework.validation.Errors;

import java.util.Locale;
import java.util.stream.Collectors;

@Service
public class PersonService {

    @Autowired
    private UserValidator userValidator;

    @Autowired
    private MessageSource messageSource;

    public void createPerson(User user) {
        Errors errors = new BeanPropertyBindingResult(user, "person");
        userValidator.validate(user, errors);

        if (errors.hasErrors()) {
            Locale locale = new Locale("uz");

            String errorMessages = errors.getAllErrors().stream()
                    .map(error -> messageSource.getMessage(error, locale))
                    .collect(Collectors.joining("; "));

            System.out.println(errorMessages);
        }

        System.out.println("Success");
    }
}
```