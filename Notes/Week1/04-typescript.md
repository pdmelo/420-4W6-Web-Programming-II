# 0.4 - TypeScript Intro

## Resources

I was going to create an exercise to introduce TypeScript, but there are already [so](https://www.typescriptlang.org/docs/) [many](https://www.w3schools.com/typescript/index.php) [resources](https://scrimba.com/learn/typescript) that have been created to do accomplish this very goal! Instead, please take 10-15 minutes to complete the following on [learn-ts.org](https://www.learn-ts.org/en/Welcome):

1. [Introduction](https://www.learn-ts.org/en/Introduction)
2. [Variables and Types](https://www.learn-ts.org/en/Variables_and_Types)
3. [Functions](https://www.learn-ts.org/en/Functions)
4. [Interfaces](https://www.learn-ts.org/en/Interfaces)

For now, we're only going to need the very basics of what TypeScript has to offer, namely, the **types**. Fear not, we'll learn more advanced TypeScript features over the course of the semester!

## *Any*thing else?

The one thing I will say for now is sometimes you will get a TS error that is fixed by changing a variable to the `any` type. Do not do this. I repeat: **DO NOT DO THIS!!** This is a huge red flag and an indicator that you're not sure what you're doing.

[This](https://thoughtbot.com/blog/typescript-stop-using-any-there-s-a-type-for-that) is a great article describing why you shouldn't use the `any` type if you're curious.

> Why shouldn't we use any again?
>
> - It yields the compiler obsolete. We're telling the compiler, "Help not needed, I got this".
> - We're passing on an opportunity to document the code as we write it.
> - Our first line of defense is down.
> - In a dynamic language, we assume that things can have any type, and the patterns we employ follow this assumption. If we start using a statically typed language as a dynamic one, then we're fighting the paradigm.
> - As we continue to make changes to the codebase, there's nothing to guide/help us.
> - With great freedom comes great responsibility (the compilers). Don't turn into a compiler, use one.

> [!WARNING]
>
> If you're ever in a situation where you can't think of any (hah) other solution than using `any`, please don't hesitate to ask me how to do it a better way. I may even dock marks if I see you using `any` in an assignment.

You have been warned.
